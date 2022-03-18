# Taller

## Objetivos
* Establecer un pipeline de CI usando GitHub Actions.
* Analizar los beneficios de configurar un pipeline de CI.


## Pre-requisitos

* Haga un fork del siguiente repositorio [https://github.com/isis3710-uniandes/react-ci](https://github.com/isis3710-uniandes/react-ci)
* Realice un clone del repositorio forkeado y ejecute los siguientes comandos:

`npm install`
`npm run start`

* Verifique el funcionamiento de la aplicación de pokemons en la siguiente url : **http://localhost:3000**


## Ejemplo básico de Github Actions

* Cree un workflow para el repositorio forkeado. Para esto cree un directorio en la raíz del repositorio con el nombre `.github/workflows`, dentro de este directorio cree el archivo `hello.yml `. El archivo `hello.yml` sólo ejecutará la impresión por consola de "Hello ISIS3710", tal como se muestra en el siguiente archivo:



```yml
name: Hello ISIS3710!

on:
  push:
    branches:
      - master

jobs:
  hello_world_job:
    runs-on: ubuntu-18.04
    steps:
      - name: Say hello
        run: |
          echo "Hello ISIS3710"
```

* Identificar y entender cada una de las lineas del archivo `hello.yml`. Para esto puede ir a la [documentación](https://docs.github.com/en/actions/reference/events-that-trigger-workflows) 

* Realizar commit y push de la workflow `hello.yml`. Luego, ir a la pestaña de Actions del repositorio forkeado y revisar que se haya ejecutado el job definido en el workflow.

* Agregue un nuevo paso (step) al job para obtener la fecha actual por consola. Recuerda que el ambiente es Ubuntu. [Documentación date](https://man7.org/linux/man-pages/man1/date.1.html)

* Realice el commit y push y verifique la ejecución del job en GitHub.


## Ejecución proyecto React con Github Actions

En esta sección, se espera configurar un workflow para instalar las dependecias del proyecto de React y ejecutar ESLint para verficar el estilo de código de la aplicación.

* Cree un nuevo archivo en el directorio `.github/workflows` con el nombre de `pipeline.yml`. Este archivo se encargará de configurar el job con los distintos pasos para hacer la instalación de dependencias y la ejecución del linter. Copie las siguientes lineas de código en el archvo pipeline.yml y realice commit y push.

```yml

name: Pipeline React

on:
  push:
    branches:
      - master

jobs:
  simple_deployment_pipeline:
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: '12.x'
      - name: npm install 
        run: npm install  
      - name: lint
        run: npm run eslint
```

* Verifique para que sirve las expresiones `uses: actions/setup-node@v1` y `- uses: actions/checkout@v2` en la documentación.
* Al ir a la pestaña de actions en Github debería aparecer varios errores en el linter y la ejecución del job debe ser fallida.
* Realice la corrección de todos los errores en la aplicación para obtener una ejecución exitosa. 

### Build
* Agregue el siguiente paso para realizar la construcción de la carpeta `build`. Esta carpeta contiene todos los js minificados y optimizados para despliegues a producción. 

```yml
  - name: Build
    run: npm run build
```
* Verifique la correcta construcción en la pestaña Actions del repositorio después de hacer commit y push de los cambios.

## Trabajando con Pull Request

Una parte interesante de GitHub Actions es que permite hacer builds de las aplicaciones no sólo cuando se hacen push a las ramas sino también cuando se abren o se actualizan nuevos Pull Request(PR). Esto permite establecer unas condiciones mínimas para aprobar o no un PR, como por ejemplo, el 100% de las pruebas existentes deben ser satisfactorias o la cobertura de pruebas no debe ser inferior a un porcentaje establecido. 
En el proyecto fork, realice los siguientes cambios para configurar el trigger del workflow cuando se abra un nuevo PR o se actualice.

```yml
  on:
  push:
    branches:
      - main
  pull_request:
    branches: [main]
    types: [opened, synchronize]
  ...
```
* Cree una nueva rama a partir de master que se llame `feature/update-readme`. En esta rama, agregue su nombre, código y correo electrónico al finalizar el readme. Cree un pull request de esta rama hacia master y verifique que el workflow empieza a ejecutar el job establecido anteriormente. 

* Una vez finalizado el job, el PR deberá tener un nuevo label donde muestre el resultado del job (Para este caso debe ser exitoso).
* Realice algún cambio en el PR que rompa la construcción (ESLint rules) suba los cambios y verifique el resultado.


### Despliegue a Heroku
En esta sección, vamos a establecer un nuevo paso (step) para realizar el despliegue a [Heroku](https://dashboard.heroku.com/).

#### Pre requisitos
* Tener una cuenta en [Heroku](https://dashboard.heroku.com/).
* Tener instalado [heroku-cli](https://devcenter.heroku.com/articles/heroku-cli) o en su defecto [heroku-npm](https://www.npmjs.com/package/heroku)

#### Configuración de Heroku
* Realice la autenticación usando heroku-cli, para esto ejecutar `heroku login`
* Cree una nueva aplicación usando el siguiente comando `heroku create --region eu {your_app_name}`
* Ahora cree un token de acceso a la aplicación de heroku creada, para esto ejecute en la consola `heroku authorizations:create`. Al finalizar este comando arrojará el token. Este token debe ser guardado en Github. Para esto ingrese al repositorio forkeado en la opción settings -> secrets. Allí cree un nuevo secreto con el nombre HEROKU_API_KEY y en el valor coloque el token.
* Adicione el archivo `procfile` en el root del repositorio (junto a package.json) con la siguiente línea `web: node app.js`.


### Github Actions 
Ahora adicione las siguientes líneas para indicarle a GitHub actions cómo hacer un despliegue a Heroku.

```yml
- uses: akhileshns/heroku-deploy@v3.12.12 # This is the action
    with:
      heroku_api_key: ${{secrets.HEROKU_API_KEY}}
      heroku_app_name: "{your_app_name}" #Must be unique in Heroku
      heroku_email: "{your_email}"
```

Para más información y opciones en el despliegue a Heroku usando la acción `akhilenhns` puede ir a la [Documentación](https://github.com/AkhileshNS/heroku-deploy)

* Realice el commit y el push y verifique la correcta construcción y despliegue en Heroku. 

* Si nos damos cuenta, cada vez que se cree un nuevo PR GitHub Actions desplegará esta versión en Heroku. Investigue como hacer para desplegar a Heroku solamente cuando se realice push a master y no cuando se cree o se actualice un PR.


Suba a BS el link del repositorio con todas las indicaciones anteriores.

















