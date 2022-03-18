En las secciones anteriores se explicó brevemente como crear un servidor web que estuviera en la capacidad de aceptar solicitudes mediante el protocolo `http`. Una capa de abstracción superior estándar la industria son los servicios web [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer), un estilo arquitectural que define la forma en la que interoperan los sistemas de computadores en internet. A través de RESTful, dos computadores están en la capacidad de compartir recursos web usando un uniforme y predeterminado conjunto de operaciones [stateless](https://en.wikipedia.org/wiki/Stateless_protocol).

Como ya se mencionó, los servicios web RESTful están basados en `http`. De este protocolo se sabe que para lograr la conexión entre dos computadores se requiere de una IP o nombre de dominio y un puerto: `http://<dominio>:<puerto>/`. Cada computador o servidor puede tener a disposición múltiples recursos que se acceden especificando un path y un método de acceso: `<metodo>: http://<dominio>:<puerto>/<resource-id>`. Este path es también conocido como [URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier) y debe ser único entre todos los recursos disponibles en el servidor.

Los métodos más comunes se describen a continuación:

- **GET:** Obtiene la información de un recurso mediante su URI. No debe ser usado para la modificación de recursos.
- **POST:** Crea un nuevo recurso en el servidor. La respuesta del servicio deberá incluir la URI del recurso recién creado.
- **PUT:** Modifica la información de un recurso.
- **DELETE:** Elimina el recurso inhabilitando su acceso mediante la URI.

Otros aspecto importante a tener en cuenta son los códigos de estado que retorna el servidor ante la solicitud de lectura, creación, modificación o eliminación de un recurso:

- **1xx:** Solicitud recibida, el proceso se encuentra en ejecución.
- **2xx:** La acción se llevó a cabo de forma satisfactoria.
- **4xx:** Error en la solicitud por parte del cliente.
- **5xx:** El servidor no está en la capacidad de procesar la solicitud.

Más información acerca de la implementación de HTTP puede ser encontrada [aquí](https://www.tutorialspoint.com/http/index.htm). Otros aspectos fundamentales a cubrir para el uso efectivo de RESTful son los tipos de contenidos que pueden ser transmitidos en el cuerpo de las solicitudes y las respuestas, los tipos de parámetros (PathParam y QueryParam) y los headers.

## Express

Node.js dispone del módulo `express` el cual habilita la creación rápida de servicios web tipo RESTful. Para poder usarlo, recuerde instalarlo mediante el comando `npm install --save express`.

El siguiente fragmento de código muestra el uso básico de `express` creando un servidor que dispone de un solo recurso ubicado en la raíz y al cuál se puede acceder mediante el método `GET`: [http://localhost:3000/](http://localhost:3000/).

```javascript
// Importa los módulos que usará la aplicación
const express = require('express');

// Crea una instancia del servidor
const app = express();

// Habilita el acceso al recurso ubicado en la raíz del servidor
app.get('/', function (req, res) {
	
  // Similar al módulo http, establece el contenido de la respuesta del servicio
  res.send('Hello World');
   
});

// Establece el puerto por el cuál escuchará el servidor
app.listen(3000, ()=>{
  console.log("Listening on port 3000");
});
```

Con este código de base, usted estará en la capacidad de desarrollar todos los recursos que requiera con acceso incluso mediante todos los métodos previamente mencionados. Automáticamente, `express` realiza el enrutamiento entre URIs y funcionalidades.

El siguiente fragmento de código habilita el acceso a un recurso `clients` a través de los métodos GET, POST, PUT y DELETE. Hasta este punto no se incluyen aspectos de persistencia de la información.

```javascript
const express = require("express");
const Joi = require("joi");

const app = express();

app.use(express.json());

const clients = [
  { id: 1, name: "Nestle" },
  { id: 2, name: "IBM" },
  { id: 3, name: "Apple" },
];

app.get("/api/clients", (req, res) => {
  res.send(clients);
});

app.get("/api/clients/:id", (req, res) => {
  const client = clients.find((c) => c.id === parseInt(req.params.id));
  if (!client)
    return res.status(404).send("The client with the given id was not found.");
  res.send(client);
});

app.post("/api/clients", (req, res) => {
  const schema = Joi.object({
    name: Joi.string().min(3).required(),
  });

  const { error } = schema.validate(req.body);

  if (error) {
    return res.status(400).send(error);
  }

  const client = {
    id: clients.length + 1,
    name: req.body.name,
  };

  clients.push(client);
  res.send(client);
});

app.put("/api/clients/:id", (req, res) => {
  const client = clients.find((c) => c.id === parseInt(req.params.id));
  if (!client)
    return res.status(404).send("The client with the given id was not found.");

  const schema = Joi.object({
    name: Joi.string().min(3).required(),
  });

  const { error } = schema.validate(req.body);

  if (error) {
    return res.status(400).send(error);
  }

  client.name = req.body.name;

  res.send(client);
});

app.delete("/api/clients/:id", (req, res) => {
  const client = clients.find((c) => c.id === parseInt(req.params.id));
  if (!client)
    return res.status(404).send("The client with the given id was not found.");

  const index = clients.indexOf(client);
  clients.splice(index, 1);

  res.send(client);
});

app.listen(3000, () => {
  console.log("Listening on port 3000");
});
```

## Reto 1

Con base en el código anterior, establezca un mecanismo de persistencia basado en archivos JSON almacenados en el servidor. El servidor debe permitir las siguientes funcionalidades:

- Crear un nuevo recurso `estudiante` con atributos como nombre, apellido, carrera y semestre. Asignarle un id único de forma incremental. Retornar la URI del recurso creado.
- Leer un estudiante previamente creado mediante su URI.
- Modificar alguno de los atributos de un estudiante específico. El cliente debe poder enviar los atributos a modificar en formato JSON en el cuerpo de la solicitud.
- Eliminar un estudiante por su URI. Este ya no debe estar disponible para consulta o modificación.

## Reto 2

- Cree un conjunto de servicios que permitan acceder a un nuevo recurso denominado `profesores`. La URI debe ser [http://localhost:3000/profesores](http://localhost:3000/profesores).
- En un nuevo servicio con URI [http://localhost:3000/personas](http://localhost:8081/personas), utilice QueryParams para realizar filtros de coincidencia parcial en los atributos nombre o apellido de los dos tipos de recursos. La respuesta del servicio debe incluir la coincidencia de ambos tipos de recursos.