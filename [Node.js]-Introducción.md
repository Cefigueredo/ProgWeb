Node.js habilita al desarrollador para crear aplicaciones del lado del servidor usando el lenguaje de programación JavaScript. Es un entorno de ejecución multiplataforma de código abierto y puede descargarse directamente desde la [página oficial](https://nodejs.org/es/download/).

Para iniciar un proyecto en node, ubíquese en una carpeta y ejecute el comando `npm init -y`. 

Luego, incluya un archivo .gitignore así: 

`wget https://raw.githubusercontent.com/github/gitignore/master/Node.gitignore -O .gitignore`

## Manipulación de archivos

```javascript
//Importa el módulo para la manipulación de archivos
const fs = require("fs");

//Lee un archivo del sistema de archivos
fs.readFile("app.js", (err, data) => {
  console.log(data);
});

//Crea un archivo en el sistema de archivos 
fs.writeFile("file.txt", "contenido", "utf-8", (err) => {
  if (err) console.log("Error writing file");
});

```


## Creación de un servidor web que escucha peticiones en el puerto 8081

```javascript
// Importa el módulo http para la creación del servidor
const http = require('http');

// Crea una nueva instancia del servidor
http.createServer(function (req, res) {

  // Encabezado de la respuesta por defecto del servidor
  res.writeHead(200, {'Content-Type': 'text/html'}); 
  
  // Contenido de la respuesta por defecto del servidor
  res.end('Hello World!');

}).listen(8081); // Puerto que usará el servidor para escuchar las solicitudes
```
Este y todos los demás fragmentos de código se encuentran disponibles en el actual repositorio.

Para iniciar el servidor abra una nueva línea de comandos y diríjase a la carpeta que acaba de crear. Posteriormente, ejecute el siguiente comando: `node server.js`. Si no se muestra ningún error en la línea de comandos, podrá realizar una petición al servidor desde cualquier navegador mediante la URL [http://localhost:8081/](http://localhost:8081/).

Este tutorial se divide en varias secciones en las cuales se describen y ejemplifican algunos de los elementos más importantes de Node.js. Al final de cada sección se plantea un pequeño reto el cuál deberá ser resuelto a partir de lo aprendido hasta el momento. Las secciones son:

* [Funcionalidades Básicas](%5BNode.js%5D-Funcionalidades-Básicas)
* [Servicios Web RESTful](%5BNode.js%5D-Servicios-Web-RESTful)
* [Persistencia](%5BNode.js%5D-Persistencia)

Toda la documentación del entorno de ejecución puede ser encontrada en la [página oficial](https://nodejs.org/es/docs/).