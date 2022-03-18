# Instalación Node.js, ESLint y Prettier

##  Node.js

Instalamos [Node.js](https://nodejs.org/en/)

Seleccionamos la versión que dice _**“LTS - Recommended for most users”**_. La diferencia entre una versión _LTS_ (Long Term Support) y una _Actual_, es que la versión LTS tiene por lo menos soporte durante 16 meses, mientras que la versión Actual solo tiene soporte durante 8 meses con features y actualizaciones constantes. Por lo cual, se considera mejor utilizar la versión LTS. 

Procedemos con la instalación de Node.js. Es importante añadir Node.js al PATH en las variables de entorno. Esta es una opción que viene en el instalador. Si no lo hicieron durante la intalación, se añade como cualquier otra variable de entorno dependiendo en donde hayan instalado Node.

Ejecutamos el siguiente comando en cualquier command prompt: _**"node -v”**_ si aparece la versión de Node, habremos instalado correctamente Node.js. Con la instalación de Node.js habremos también instalado el gestor de paquetes `npm`, que puede ser verificado con la línea _**“npm -v"**_.

![Node -v](https://i.imgur.com/646gws0.png)

```diff
! Nota: Tmbién se puede utilizar Yarn en lugar de npm.
```
Instalamos las dependencias de prettier y eslint. Esto se hace por cada proyecto. Para esto se paran en la carpeta root del proyecto y abren una terminal.

* Para npm: en el proyecto que deseemos instalar ESLint, ejecutamos _**“npm i prettier eslint-config-prettier –save-dev” **_

* Para Yarn: en el proyecto que deseamos instalar ESLint, ejecutamos _**“yarn add --dev prettier eslint-config-prettier” **_

## Instalación ESLint para Visual Studio Code (Marcado de errores)

Abrimos el Marketplace de Visual studio code

![market place](https://i.imgur.com/NXvrycQ.png)

Buscamos ESLint v. 2.1.8 (de Dirk Naeumer) y lo instalamos.

Ahora en nuestra carpeta del proyecto (en root) creamos un archivo denominado **_“.eslintrc.json”_**. En este archivo van las configuraciones de eslint.

Aunque se pueden añadir varias configuraciones, una configuración recomendada es la siguiente:

### Configuración ESLint recomendada

```
{
	  "env": {
	    "browser": true,
	    "commonjs": true,
	    "es6": true,
	    "node": true
	  },
	  "extends": "eslint:recommended",
	  "globals": {
	    "Atomics": "readonly",
	    "SharedArrayBuffer": "readonly"
	  },
	  "parserOptions": {
	    "ecmaVersion": 2018,
	    "sourceType": "module"
	  },
	  "rules": {
	    "indent": ["warn", 2, { "SwitchCase": 1 }],
	    "linebreak-style": ["error", "windows"],
	    "quotes": ["error", "double"],
	    "semi": ["error", "always"],
	    "no-console": "warn",
	    "no-var":"error",
	    "no-undef": 0,
            "no-alert": "error",
            "curly": "error",
            "space-before-blocks": ["warn", "always"],
            "no-loop-func": "error",
            "key-spacing": ["warn", {"beforeColon": false, "afterColon": true}]
          }
}

```

Dado el caso en que utilicen MacOS, deberán cambiar el valor de atributo **linebreak-style** de **windows** a **unix**.

```diff
+ Importante: Reiniciamos VS code. El plugin del Marketplace permanece en el IDE, no obstante los comandos para npm o yarn se tienen que hacer por cada proyecto (a menos que lo instalen global, sin embargo, muchos proyectos tienen diferentes ESLint y prettier diferentes por lo cual no es recomendado).
```

## Instalación prettier para Visual Studio Code (Identación y _beautify_ del código)

1. Abrimos el marketplace de visual studio code

![Market place](https://i.imgur.com/GCZeX4G.png)

2. Buscar Prettier e instalarlo (Prettier – Code formatter)

3. Poner Prettier como default formatter.

Para esto oprimimos Ctrl/cmd + shift + p -> Buscamos “Settings” y seleccionamos la opción: Preferences: Open Settings (JSON).

![Settings (JSON)](https://i.imgur.com/wdDY2vW.png)

Seguido de esto nos aparecerá un archivo JSON con las configuraciones de VS code. 

Poner las siguientes líneas de código

```
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}

```

El editor.defaultFormatter aplicará para todos los lenguajes. Si quieren poner lenguaje por lenguaje los ponen entre brackets: ([html], [css], etc…)

Para identar cada vez que se guarde un archivo se realiza lo siguiente: 

Se abre Ctrl/Cmd + shift + P -> Prefrences: Open Workspace Settings

![Workspace preferences](https://i.imgur.com/iOVmPv3.png)

Buscan **"Format on save"** y le dan en el checkbox que aparece

![Format on save](https://i.imgur.com/fOGxtSA.png)


4. Creamos un archivo .prettierrc.json

En este archivo van las configuraciones de prettier, para efectos del curso una de las configuraciones es:

```
{
	  "semi": true,
	  "tabWidth": 2,
	  "singleQuote": false,
	  "trailingComma": "all"
}

```

```diff
+ Importante: Reiniciamos VS code.
```

Si hicieron todo el procedimiento correcto, podrán ver errores en los archivos javascript del proyecto y al hacer CTRL/CMD + SHIFT + F verán como se reorganiza su código o al guardarlo. 

```diff
! Nota: También pueden utilizar beautify para el HTML
```


