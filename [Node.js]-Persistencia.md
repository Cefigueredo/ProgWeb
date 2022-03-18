El presente tutorial está desarrollado usando la base de datos in-memory [SQLite](https://www.sqlite.org/index.html). Este motor tiene la ventaja de no requerir instalación en el ambiente local. Basta con que copie los recursos en la ruta de su preferencia tal como se muestra [aquí](http://www.sqlitetutorial.net/download-install-sqlite/). Si lo prefiere, puede hacer uso de la GUI [SQLite Studio](https://sqlitestudio.pl/index.rvt) la cual también funciona sin requerir instalación.

Un aspecto que debe tener en cuenta es que SQLite, por defecto, no requiere de credenciales para permitir la conexión. Si está usando un motor de base de datos como MySQL, [aquí](https://www.w3schools.com/nodejs/nodejs_mysql.asp) le muestran como crear y pasar el objeto que incluye información como IP, puerto, nombre de base de datos, usuario y contraseña, necesarios para establecer conexión con un motor de base de datos tradicional. 

En la sección anterior se abordó el problema del acceso a un recurso mediante su URI y durante el reto propuesto usted estableció un mecanismo de persistencia básico basado en archivos JSON. Los siguientes fragmentos de código habilitan al servidor a administrar los recursos de tipo `estudiante`, esta vez haciendo uso de una base de datos relacional. 

En primera instancia, desde la consola de línea de comandos de SQLite cree una base de datos denonimada `uniandes` y luego la tabla `estudiante`. La consola de línea de comandos se abrirá al ejecutar el archivo `sqlite3.exe`. Esta [guía](http://www.sqlitetutorial.net/sqlite-commands/) le muestra los comandos principales que debe tener en cuenta. El código para la creación de la tabla es el siguiente:

```sql
CREATE TABLE estudiante (
  id       INT          PRIMARY KEY AUTOINCREMENT,
  nombre   VARCHAR (30),
  apellido VARCHAR (30),
  carrera  VARCHAR (30),
  semestre INT
);

```

Si lo prefiere, también puede usar SQLite Studio para la creación de la tabla. En cualquier caso, asegúrese de crear la base de datos en una carpeta llamada `db` en el mismo directorio en el que se encuentra el ejecutable de SQLite (por ejemplo, `C:\sqlite\db`).

El siguiente fragmento de código es una variación del presentado en la sección anterior. En este se utiliza el módulo denominado `sqlite3` el cuál permite acceder al motor de base de datos. Cada uno de los métodos creados para el recurso `estudiante` han sido modificados con su correspondiente función CRUD para hacer SELECT, INSERT, UPDATE o DELETE sobre los registros de la tabla `estuadiante` previamente creada:

```javascript
// Importa los módulos que usará la aplicación
var express = require('express');
var sqlite3 = require('sqlite3').verbose();

// Crea una instancia del servidor
var app = express();

// Habilita el uso de contenidos tipo JSON en las solicitudes
app.use(express.json());

// Habilita el acceso al recurso ubicado en la raíz del servidor
app.get('/', function (req, res) {
	
  // Similar al módulo http, establece el contenido de la respuesta del servicio
  res.send('Hello World');
   
});

// Habilita el acceso mediante GET al recurso estudiante con id enviado como parámetro
app.get('/estudiante/:id', function (req, res) {
  
  // Permite obtener el parámetro indicado en el path
  var id = +req.params.id;
  
  // Crea la conexión con la base de datos 
  var db = new sqlite3.Database('c:\\sqlite\\db\\uniandes.db');

  // Crea la sentencia SQL para realizar la consulta
  db.get(`SELECT id, nombre, apellido, carrera, semestre FROM estudiante WHERE id = ?`, 
    [id], function(err, row) {
    
    // Si ocurre algún error durante la consulta, retorna un error con código 500
    if (err) res.status(500).send('El servidor no pudo procesar la solicitud');
    
    // Establece el contenido de la respuesta del servicio
    res.send(row);

  });
 
  // Cierra la conexión con la base de datos
  db.close();
   
});

// Habilita el acceso mediante POST al recurso estudiante
app.post('/estudiante', function (req, res) {
  
  // Almacena en la variable el contenido de la solicitud
  var estudiante = req.body;

  // Crea la conexión con la base de datos 
  var db = new sqlite3.Database('c:\\sqlite\\db\\uniandes.db');

  // Crea la sentencia SQL para realizar la inserción
  db.run(`INSERT INTO estudiante(nombre,apellido,carrera,semestre) VALUES(?,?,?,?)`, 
    [estudiante.nombre, estudiante.apellido, estudiante.carrera, estudiante.semestre], function(err) {
    
    // Si ocurre algún error durante la inserción, retorna un error con código 500
    if (err) res.status(500).send('El servidor no pudo procesar la solicitud');

    // Construye la URI del recurso que se está creando
    var uri = req.originalUrl + '/' + this.lastID;
    
    // Establece el contenido de la respuesta del servicio
    res.send('Creación de un nuevo estudiante con URI ' + uri);

  });
 
  // Cierra la conexión con la base de datos
  db.close();
   
});

// Habilita el acceso mediante PUT al recurso estudiante con id enviado como parámetro
app.put('/estudiante/:id', function (req, res) {
  
  // Permite obtener el parámetro indicado en el path
  var id = req.params.id;
  
  // TODO: Implementación del UPDATE a la base de datos

  // Similar al módulo http, establece el contenido de la respuesta del servicio
  res.send('Modifica el estudiante con id ' + id);
   
});

// Habilita el acceso mdiante DELETE al recurso estudiante con id enviado como parámetro
app.delete('/estudiante/:id', function (req, res) {
  
  // Permite obtener el parámetro indicado en el path
  var id = req.params.id;
  
  // TODO: Implementación del DELETE a la base de datos

  // Similar al módulo http, establece el contenido de la respuesta del servicio
  res.send('Elimina el estudiante con id ' + id);
   
});

// Establece el puerto por el cuál escuchará el servidor
var server = app.listen(8081);
```

Como se puede observar, hasta el momento solo se ha implementado el acceso a la base de datos correspondiente a los métodos GET y POST los cuáles podrán ser probados mediante las URIs [http://localhost:8081/estudiante/1](http://localhost:8081/estudiante/1) y [http://localhost:8081/estudiante](http://localhost:8081/estudiante), respectivamente. Adicionalmente, para la ejecución del método POST la solicitud debe incluir el encabezado `Content-Type: application/json`, el cuál le indica al servidor que se enviará en el cuerpo de la solicitud información en formato JSON como la siguiente:

```javascript
{
  "nombre": "andres",
  "apellido": "perez",
  "carrera": "sistemas",
  "semestre": 3
}
```

Si todo va bien, la respuesta del método GET incluirá un JSON como el anterior. Por otro lado, la respuesta del método POST incluirá la URI del recurso recién creado.

## Reto 1

Con base en el código anterior:

- Implemente el acceso a la base de datos para los dos métodos faltantes: PUT y DELETE.
- Cree un servicio adicional con URI GET: [http://localhost:8081/estudiantes](http://localhost:8081/estudiantes) que le permita listar todos los estudiantes almacenados en la tabla y que adicionalmente acepte una QueryParam para realizar filtro por coincidencia parcial sobre las columnas de nombre y apellido.

## Reto 2

- Cree un conjunto de servicios similares al anterior para la entidad `profesor`. La URIs deben ser [http://localhost:8081/profesor](http://localhost:8081/profesor) y [http://localhost:8081/profesores](http://localhost:8081/profesores).
- Cree en la base de datos una nueva tabla denominada `asignatura` y establezca un modelo relacional adecuado para relacionarla con las tablas `estudiante` y `profesor`. Un estudiante puede ver múltiples asignaturas y una asignatura puede ser tomada por múltiples estudiantes. Un profesor puede dictar múltiples asignaturas pero una asignatura solo puede ser dictada por un profesor. Implemente un servicio que permita responder a la pregunta: ¿Qué estudiantes están viendo clase con el profesor Juan? Se debe mostrar el detalle de la asignatura que relaciona al estudiante con el profesor.