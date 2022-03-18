Como en cualquier lenguaje de programación o framework, existe un conjunto de módulos con funciones comunes que agilizan el desarrollo. Una descripción de los módulos básicos que implementa Node.js puede ser encontrada en la página de la [W3C](https://www.w3schools.com/nodejs/ref_modules.asp).

En la sección de [Introducción](%5BNode.js%5D-Introducción) se muestra brevemente la puesta en marcha de un servidor web utilizando el módulo `http`. [Http](https://es.wikipedia.org/wiki/Protocolo_de_transferencia_de_hipertexto) es el protocolo estándar para la comunicación entre recursos en la web y, entre otras cosas, es la base para la construcción de servicios REST, tema que se abordará en la sección siguiente.

Otro modulo de importante utilidad es el `fs`, el cual permite el acceso a archivos almacenados en el disco local en el que se encuentra desplegado el servidor. Estos archivos pueden ser útiles para persistir información no estructurada de los usuarios o variables de configuración como rutas de acceso a servicios de terceros, lenguaje por defecto de la aplicación y credenciales de acceso a base de datos. A continuación se muestra un ejemplo de su uso:

```javascript
// Importa los módulos que usará la aplicación
var http = require('http');
var fs = require('fs');

// Crea una nueva instancia del servidor
http.createServer(function (req, res) {
  
  // Lee el archivo testfile.txt el cual se encuentra en la misma ruta que este script
  fs.readFile('testfile.txt', 'utf8', function(err, data) {
    if (err) throw err; // Retorna error si no encuentra el archivo
	
    // Encabezado de la respuesta del servidor
    res.writeHead(200, {'Content-Type': 'text/html'}); 
  
    // Especifica el contenido que debe ser incluido en la respuesta
    res.end(data);
  });

}).listen(8080); // Puerto que usará el servidor para escuchar las solicitudes
```
Recuerde que para iniciar el servidor, desde la línea de comandos debe ejecutar el comando `node server.js` y que podrá probar la aplicación mediante la URL [http://localhost:8080/](http://localhost:8080/).

## NPM

Node.js tiene un extenso número de módulos / paquetes que han sido desarrollados y mantenidos por la comunidad de desarrolladores desde su lanzamiento en 2009. Para poder tener acceso a estos, con la instalación de Node.js se habilita el gestor de  paquetes [NPM](https://www.npmjs.com/), un recurso de línea de comandos para administrar todos los módulos / paquetes que se encuentran habilitados en los diferentes repositorios soportados.

La forma en la que se instala un nuevo paquete es ejecutando el siguiente comando: `npm install <nombre_del paquete>`. Si no obtiene ningún error, en la ruta raíz de su proyecto se debe crear una carpeta llamada `node_modules` con todas las dependencias del módulo que acaba de instalar.

El siguiente fragmento de código es muy similar al presentado al inicio de esta sección. Se ha incluido el módulo `upper-case` que usted deberá instalar mediante el comando `npm intall upper-case`.

```javascript
// Importa los módulos que usará la aplicación
var http = require('http');
var fs = require('fs');
var uc = require('upper-case');

// Crea una nueva instancia del servidor
http.createServer(function (req, res) {
  
  // Lee el archivo testfile.txt el cual se encuentra en la misma ruta que este script
  fs.readFile('testfile.txt', 'utf8', function(err, data) {
    if (err) throw err; // Retorna error si no encuentra el archivo
	
    // Encabezado de la respuesta del servidor
    res.writeHead(200, {'Content-Type': 'text/html'}); 
  
    // Especifica el contenido que debe ser incluido en la respuesta
    // Note que el objeto data ahora es parámetro de la función uc, alias del módulo upper-case
    res.end(uc(data));
  });

}).listen(8080); // Puerto que usará el servidor para escuchar las solicitudes
```

Una buena práctica para el desarrollo de una aplicación de Node.js es el uso del archivo `package.json`. En este archivo se encuentra meta-información asociada a la aplicación en desarrollando: nombre, versión, autor, licencia, dependencias, entre otros. De esta forma, cualquier persona deberá estar en la capacidad de ejecutar su código al disponer del detalle, por ejemplo, de las dependencias (módulos / paquetes) que se deben instalar previamente. NPM tiene una funcionalidad para la creación del archivo `package.json`. Abra una consola de línea de comandos y diríjase a la ruta raíz de su proyecto. Una vez allí ejecute el siguiente comando: `npm init` e ingrese la información allí solicitada.

Puede realizar una variación sobre el comando usado para instalar un nuevo módulo: `npm install --save <nombre_del paquete>`. El parámetro `--save` agregará automáticamente la dependencia al archivo `package.json`, habilitandolo para compartir el código desarrollado e informar acerca de las dependencias requeridas para su adecuado funcionamiento. Para instalar todas las dependencias allí dispuestas, basta con ejecutar el comando `npm install`.

## Reto

Cree un servidor web que esté en la capacidad de devolver las palabras de un archivo de texto que coincidan de forma parcial con un [QueryParam](https://en.wikipedia.org/wiki/Query_string) enviando durante la solicitud. Las palabras deben incluir su frecuencia similar a como se muestra a continuación:

```javascript
{
  "casa": 5,
  "perro": 8,
  "foca": 2
}
```