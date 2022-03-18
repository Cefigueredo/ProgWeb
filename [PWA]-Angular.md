### Herramientas

En este tutorial se creará una versión inicial de una PWA. Para ello se necesita tener previamente instalado Angular `npm install -g @angular/cli`) y un servidor web (`npm install -g http-server`).

Las pruebas se harán con las herramientas de desarrollo de Google Chrome. 

### Inicio

Cree un nuevo proyecto Angular con `ng new jokesapp`.

Ingrese a la carpeta `jokesapp` y compile la aplicación para un entorno de producción con el comando `ng build --prod`. Esto debe crear una nueva carpeta denominada `dist/jokesapp`.

Ingrese a esa carpeta y ejecute un servidor web en ella con el comando `http-server -o`.

Esto desplegará en el browser la aplicación por defecto provista por Angular. 

Abra las herramientas de desarrollo de su navegador y verifique en la pestaña Application que aún no hay soporte para PWA (ver opción _Service Workers_).

Para comprobarlo, marque la casilla _Offline_, y deberá ver el _Downsaur_.


### Agregando soporte para PWA

Ingrese a la consola y ejecute el comando `ng add @angular/pwa`. Esto deberá agregar varios archivos a su proyecto. Por ejemplo: `src/manifest.json`, `ngsw-confing.json`. También la carpeta `src/assets/icons`, junto con la actualización de los archivos `angular.json`, `package.json`, `src/app/app.module.ts` y `src/index.html`.

Recompile la aplicación para un entorno de producción, inicie el servidor web y mire el comportamiento en las herramientas de desarrollo. Debe ver que ya existe un Service Worker, y si pone el navegador fuera de línea, la applicación continúa apareciendo en el cache de navegador.

### Manejando updates

Modifique el archivo `src/app.component.html`, por ejemplo, cambiando el encabezado de primer nivel así:  `<h1>¡¡¡ Welcome to {{ title }}!!!</h1>`.

Recompile la aplicación para un entorno de producción, inicie el servidor web y es posible que la página no se actualice a la nueva versión. Por tanto es necesario forzar la limpieza del cache y hacer un _hard reload_ de la página. Para esto, haga click derecho en el ícono de actualización que está en la parte izquierda de la barra de direcciones. 

Para forzar la actualización de la aplicación modifique el archivo `src/app/app.component.ts` asi:

```
import { Component } from '@angular/core';
import { SwUpdate } from "@angular/service-worker";

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'pwa';

  constructor(updates: SwUpdate){
  	updates.available.subscribe(event => {
  		updates.activateUpdate().then(() => {
  			document.location.reload();
      });
  	});
  }
}
```

Luego de esto, realice alguna modificación a `src/app.component.html`. Recompile la aplicación para un entorno de producción, inicie el servidor web y espere unos segundos para ver la actualización de la página. 


### Agregando funcionalidades adicionales

Tomando como referencia lo visto en las clases anteriores, realice lo siguiente:

* Cree un nuevo módulo denominado `joke`.
* Cree un nuevo componente denominado `joke-item`
* Cree un nuevo servicio para conectarse con este endpoint: https://api.chucknorris.io/jokes/random, el cual retorna un objeto que contiene un chiste aleatorio sobre Chuck Norris
* En la vista del componente `joke-item` muestre el valor obtenido en el objeto retornado por el servicio, en su atributo `value`.
* Asegúrese de importar y exportar los módulos, componentes y servicios requeridos.


Para dar soporte final a la interacción sin conexión, modifique el archivo `index.html` y agregue está línea en el encabezado:

```
<link href="https://fonts.googleapis.com/css?family=Montserrat" rel="stylesheet">
```

Incluya el siguiente código en el archivo `styles.css`:

```css
body, html {
	height: calc(100% - 4em);
}

body {
	background: brown;
	color: #fff;
	text-align: center;
	display: grid;
	align-items: center;
	font-family: 'Montserrat';
	font-size: 3em;
	padding: 2em;
}
```

Modifique el archivo `src/ngsw-config.json` así:


```
 "dataGroups": [{
    "name": "jokes-api",
    "urls": ["https://api.chucknorris.io/jokes/random"],
    "cacheConfig":  {
      "strategy": "freshness",
      "maxSize": 20,
      "maxAge" : "1h",
      "timeout": "5s"
    }
  }]
```

Recompile la aplicación para un entorno de producción, inicie el servidor web y espere unos segundos para ver la actualización de la página. 

Finalmente, ponga el navegador fuera de línea y actualice la página para ver el comportamiento.






