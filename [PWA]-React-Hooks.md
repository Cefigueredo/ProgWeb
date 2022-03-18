### Herramientas

En este tutorial se creará una versión inicial de una PWA en react. Para ello se necesita tener previamente instalado un servidor web, por ejemplo: `npm install -g http-server`.

Las pruebas se harán con las herramientas de desarrollo de Google Chrome. 

### Inicio

Cree un nuevo proyecto en React con `npx create-react-app jokesapp --template cra-template-pwa`.

Ingrese a la carpeta `jokesapp` y compile la aplicación para un entorno de producción con el comando `npm run build`. Esto debe crear una nueva carpeta denominada `build/`.

Ingrese a esa carpeta y ejecute un servidor web en ella con el comando `http-server -o`.

Esto desplegará en el browser la aplicación por defecto provista por React. 

Abra las herramientas de desarrollo de su navegador y verifique en la pestaña Application que aún no hay soporte para PWA (ver opción _Service Workers_).

Para comprobarlo, marque la casilla _Offline_, y deberá ver el _Downsaur_.

### Agregando soporte para PWA

Vaya al archivo `src/index.js` y cambie la línea 18 por `serviceWorkerRegistration.register();` 

Recompile la aplicación para un entorno de producción, inicie el servidor web y mire el comportamiento en las herramientas de desarrollo. Debe ver que ya existe un Service Worker, y si pone el navegador fuera de línea, la applicación continúa apareciendo en el cache de navegador.

### Manejando updates

Modifique el archivo `public/index.html`, por ejemplo, cambiando el título de la página.

Recompile la aplicación para un entorno de producción, inicie el servidor web y es posible que la página no se actualice a la nueva versión. Por tanto es necesario forzar la limpieza del cache y hacer un _hard reload_ de la página. Para esto, haga click derecho en el ícono de actualización que está en la parte izquierda de la barra de direcciones y seleccione `Empty Cache and Hard Reload`. 

### Agregando funcionalidades adicionales

Tomando como referencia lo visto en las clases anteriores, realice lo siguiente:

* Cree un nuevo componente denominado `Joke`
* Cree un hook de estado para un atributo denominado `joke`
* En un hook de efecto haga un fetch al endpoint: https://api.chucknorris.io/jokes/random, el cual retorna un objeto que contiene un chiste aleatorio sobre Chuck Norris
* Cambie el estado del componente, asignando al atributo `joke` lo retornado por _fetch_ en el atributo `value`.
* En la vista del componente incluya un encabezado de primer nivel con el texto Joke, y un párrafo en el que se muestre el valor del atributo `joke` del estado. 

Modifique el archivo `public/index.html` y agregue está línea en el encabezado:

```html
<link href="https://fonts.googleapis.com/css?family=Montserrat" rel="stylesheet">
```

Incluya el siguiente código en el archivo `styles.css`:

```css
body {
  margin: 0;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background: brown;
	color: #fff;
	text-align: center;
	display: grid;
	align-items: center;
	font-family: 'Montserrat';
	font-size: 3em;
	padding: 2em;
}

body, html {
	height: calc(100% - 4em);
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
    monospace;
}
```
Recompile la aplicación para un entorno de producción y visualice los resultados.

Para garantizar que siempre se puede visualizar algo en la página así se pierda conexión, modifique el hook de efecto del componente así:

```javascript
useEffect(()=>{
        if(!navigator.onLine){
            if(localStorage.getItem("joke") === null) {
                setJoke("Loading...")
            } else {
                setJoke(localStorage.getItem("joke"));
            }
        } else {
            const URL = "https://api.chucknorris.io/jokes/random";
            fetch(URL).then(res=>res.json()).then(res=>{
                setJoke(res.value);
                localStorage.setItem("joke", res.value);
            })
        }
    }, []);
```

Recompile la aplicación para un entorno de producción y visualice los resultados.

### Reto

Usando el api de Marvel (https://developer.marvel.com/) desarrolle una PWA en React que mueste información sobre los personajes de marvel. 
 
En primer lugar debe registrarse en la [página de Marvel](https://www.marvel.com/signin?referer=https%3A%2F%2Fdeveloper.marvel.com%2F) para poder obtener una llave pública y una llave privada. Una vez haya ingresado con sus credenciales, ingrese a la [documentación del api](https://developer.marvel.com/docs) para conocer los endpoints disponibles y su funcionamiento. En este caso debe utilizar el endpoint que sirve la información de los personajes (https://gateway.marvel.com:443/v1/public/characters...).

Recuerde que en la URL del endpoint debe pasar 3 parámetros:

* ts: una cadena de texto larga, por lo general se refiere a un *timestamp* u otra cadena que puede cambiar con el tiempo
* hash: el hash md5 de la concatenación del ts, la llave privada y la llave pública. hash = md5 (ts + private key + public key)
* apikey: la llave pública que le fue asignada

Para obtener información más detallada de la autorización del api de Marvel revisar esta [documentación](https://developer.marvel.com/documentation/authorization).  
Para conocer el funcionamiento de las imágenes del api de Marvel revisar esta [documentación](https://developer.marvel.com/documentation/images).

El funcionamiento de un request (probado en Postman) se ve así:

![API Marvel Request](https://i.ibb.co/2kgW0w4/apimarvel.png)













