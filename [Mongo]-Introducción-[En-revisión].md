# MongoDB
[MongoDB](https://www.mongodb.com/) es una poderosa base de datos no SQL de propósito general orientada a documentos.
MongoDB persiste los documentos en un formato [BSON](https://www.mongodb.com/json-and-bson), que es muy similar a JSON.
Una de las razones de [por qué MongoDB es tan popular](https://www.mongodb.com/blog/post/why-mongodb-popular) es gracias a su alta escalabilidad.

En este tutorial integraremos MongoDB y Express.

## Primeros pasos

- Instale globalmente el paquete express-generator `npm install -g express-generator`.

- Cree una nueva aplicación con express-generator: `express --no-view --git expressapi`.

- Ingrese a la carpeta `expressapi` e instale las dependencias: `npm install`

- Edite el archivo `package.json` y modifique la línea 6 así: `"start": "nodemon ./bin/www"`. (Asegurese de tener instalado globalmente nodemon `npm i -g nodemon`)

- Instale el módulo mongodb con el comando `npm install mongodb --save`.

### Para desarrollo local:

Si piensa usar MongoDB de manera local, debe además de lo anterior:

- Instalar MongoDB (https://www.mongodb.com/download-center/community).

- Instalar Mongo Compass (https://www.mongodb.com/products/compass).

Tenga presente que para el desarrollo local, debe estar corriendo el driver de Mongo.

## Importar un JSON a una colección

Si está usando MongoDB local, puede importar un archivo JSON desde una consola ingresando el siguiente comando:

`mongoimport --db basededatos --collection coleccion --file .\archivo.json --jsonArray`

## Integrando MongoDB con Express

### Preparar la conexión

El primer paso para acceder a MongoDB desde Express es realizando la conexión. Tenga presente que esta será necesaria para todas las demás operaciones.

Para realizar la conexión se requiere un MongoClient y la URI de su MongoDB. Para el desarrollo local, MongoDB usa por defecto el puerto 27017, pero para MongoDBs desplegadas en la nube ([Mongo Atlas](https://www.mongodb.com/cloud/atlas)) se debe usar la URI allí especificada. Si desea conocer cómo generar dicha URI, puede basarse en [Connect to MongoDB](https://docs.mongodb.com/guides/server/drivers/), bajo la pestaña Cloud.

Es buena práctica almacenar dicha variable como una [variable de entorno](https://www.npmjs.com/package/dotenv), con el fin de evitar de que por error sea publicada en GitHub y que personas externas se aprovechen de esto.

Un paso importante para garantizar la integridad de Mongo con Express es modularizar todo lo relacionado a la base de datos en un archivo independiente. Cree un nuevo archivo `lib/utils/mongo.js` para configurar la conexión a la base de datos  e importe los siguientes elementos:

```javascript
const MongoClient = require('mongodb').MongoClient;
const uri = 'mongodb://localhost:27017/';
```

Ahora cree function llamada MongoUtils() para comenzar a crear conexión a la base de datos:

```javascript
// ..
const MongoClient = require('mongodb').MongoClient;
const uri = 'mongodb://localhost:27017/';

function MongoUtils() {
  const mu = {};

  // Esta variable almacenará la conexión a MongoDB.
  // Tenga presente que es una promesa que deberá ser resuelta.
  mu.conn = MongoClient.connect( uri,  {useNewUrlParser: true, useUnifiedTopology: true });
  }
module.exports = MongoUtils();
  // ...
```


De esta manera, si en futuro requiere realizar operaciones desde Express con la base de datos, bastará con importar el módulo que acabamos de crear:

```javascript
mu = require('lib/utils/mongo.js');
```
### Creación de API 

Se desea crear un API Rest para el recurso producto. Se espera crear un servicio para obtener los productos y otro para insertar un nuevo producto a la base de datos de Mongo.

[GET] http://localhost:3000/products
[POST] http://localhost:3000/products

Para esto cree el archivo controllers/product.js el cual contendrá toda la lógica entre el cliente de Mongo y la base de datos. Tenga en cuenta que hemos importado el archivo mongo.js el cual exporta un objeto con la conexión. 
```javascript
const mdbconn = require('../lib/utils/mongo.js');

function getProducts() {
  return mdbconn.conn.then((client) => {
    return client.db('isis3710').collection('productos').find({}).toArray();
  });
}

function insertProduct(product) {
  return mdbconn.conn.then((client) => {
    return client.db('isis3710').collection('productos').insertOne(product); // Si no se provee un ID, este será generado automáticamente
  });
}

module.exports = [getProducts, insertProduct];
```

Cree el archivo products.js el cual se encargará de manejar las rutas para el recurso producto.

```javascript
var express = require('express');
var router = express.Router();
var [getProducts, insertProduct] = require('../controllers/product');

/* GET product listing. */
router.get('/', async function (req, res, next) {
  const products = await getProducts();
  console.warn('products->', products);
  res.send(products);
});
/**
 * POST product
 */
router.post('/', async function (req, res, next) {
  const newProduct = await insertProduct(req.body);
  console.warn('insert products->', newProduct);
  res.send(newProduct);
});

module.exports = router;
```

Finalmente, adicione el nuevo router al archivo app.js. 

```javascript
var express = require('express');
var path = require('path');
var cookieParser = require('cookie-parser');
var logger = require('morgan');

var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');
var productsRouter = require('./routes/products');

var app = express();

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', indexRouter);
app.use('/users', usersRouter);
app.use('/products', productsRouter);

module.exports = app;
```

### Algunas operaciones CRUD:

#### Insertar un documento

```javascript
mu.conn.then((client) => {
    client.db("Population")
      .collection("cities")
      .insertOne({ name: "Medellín" }) // Si no se provee un ID, este será generado automáticamente
      .finally(() => client.close()); // Es buena práctica de seguridad cerrar la conexión al final de cada operación
});
```

#### Obtener documentos de una colección

```javascript
// Obtiene todos los documentos de la colección cities
mu.conn.then((client) => {
    client.db("Population")
      .collection("cities")
      .find({}) // El JSON vacío indica que se están pidiendo todos los documentos
      .toArray((err, data) => {
        console.log(data);
    }).finally(() => client.close());
});
```

#### Actualizar un único documento
En este ejemplo se modifica un único documento: el que tiene el ID «idgenerico123». Para trabajar con IDs en MongoDB desde Node.js es necesario importar y usar «ObjectID». La documentación completa sobre filtros y cambios para la función updateOne la puede encontrar [aquí](https://docs.mongodb.com/drivers/node/fundamentals/crud/write-operations/change-a-document).

```javascript
const ObjectID = require("mongodb").ObjectID;

// Modifica el atributo name de un documento específico
mu.conn.then((client) => {
    client.db("Population")
      .collection("cities")
      .updateOne(
        { _id: new ObjectID("idgenerico123") }, // Filtro al documento que queremos modificar
        { $set: { name: "Envigado" } } // El cambio que se quiere realizar
      )
      .finally(() => client.close())
    });
```
* Si se usa updateOne con un filtro que haga match con más de un documento, MongoDB sólo modificará el primero. Para modificar varios documentos simultáneamente puede usar la función [updateMany()](https://docs.mongodb.com/drivers/node/usage-examples/updateMany).

#### Eliminar un documento:

```javascript
// Elimina un documento que cumpla con tener más de 8.000.000 de habitantes.
mu.conn.then((client) => {
    client.db("Population")
      .collection("cities")
      .deleteOne(
        { population: { $gt: 8000000 } }
      )
      .finally(() => client.close())
    });
```
* De manera similar como sucede con updateOne, deleteOne sólo eliminará el primer documento que cumpla la consulta indicada. Para eliminar varios documentos simultáneamente puede usar la función [deleteMany()](https://docs.mongodb.com/drivers/node/usage-examples/deleteMany).

# Aggregation framework

Está basado en el concepto de procesar pipelines. Los documentos entran a etapas de múltiples pipelines que transforman el documento a un valor agregado.
Las operaciones agregadas agrupan valores de múltiples documentos de manera conjunta, y pueden realizar varias operaciones en los datos agrupados para retornar un solo resultado.
Existen varios tipos de operaciones agregadas, sin embargo las más usadas son $match, $group, .count(), .distinct(), $lookup, $project, $unwind y $sort.
Para realizar un aggregate tenemos que hacerlo sobre una colección. Con uno de los ejemplos anteriores tendríamos

```javascript
mu.conn.then((client) => {
    client.db("Population")
      .collection("cities")
      .aggregate([
])	
      .toArray((err, data) => {
        console.log(data);
    }).finally(() => client.close());
});

```

Para cada pipeline del aggregate se necesita tener corchetes ({}) encerrando el pipeline y cada vez que se utiliza un pipeline también necesita estar encerrado en corchetes ({}). Por ejemplo
```javascript
{  $match: {}  }
```
Al final del aggregate necesitamos devolver el resultado como arreglo, haciendo .toArray() 

## $Match

El pipeline sirve para realizar lo mismo que un .find(). Sin embargo, ya que las busquedas de aggregate son más complejas nos permite hacer busquedas más complejas.

```javascript
mu.conn.then((client) => {
    client.db("Population")
      .collection("cities")
      .aggregate([
{ $match: { name: ‘Envigado’ } }
])	
      .toArray((err, data) => {
        console.log(data);
    }).finally(() => client.close());
});
```

## $group

Sirve para agrupar elementos que tengan un valor en común según campos dados. Así mismo, podemos utilizar $sum para contar cuantos documentos cumplen con el valor. $group siempre recibe un _id, sin embargo no tiene que ser obligatoriamente el _id del objeto, puede ser cualquier campo siendo este el campo por el que se va a agrupar.
```javascript
mu.conn.then((client) => {
    client.db("Population")
      .collection("cities")
      .aggregate([
{ $match: { departamento: ‘Antioquia’ } },
{
    $group : {
       _id : { $dateToString: { format: "%Y-%m-%d", date: "$createdAt" } },
       count: { $sum: 1 }
    }
  },

 } }  
])	
      .toArray((err, data) => {
        console.log(data);
    }).finally(() => client.close());
});
```

Primero, se puede ver que está el campo como $createdAt, ya que se refiere a un campo del documento. En este caso, agrupamos las ciudades que fueron creadas en la db cuyo formato en la base de datos está como **"createdAt" : ISODate("2014-03-01T08:00:00Z")** por un día específico, por ejemplo (2014-04-04
). Este formato corresponde al formato de Date de Javascript. Para pasar una fecha a un formato específico pueden ver el siguiente enlace [https://docs.mongodb.com/manual/reference/operator/aggregation/dateToString/](https://docs.mongodb.com/manual/reference/operator/aggregation/dateToString/). Por último, contamos cuántas ciudades fueron creadas en una misma fecha con **$sum**.

## .count() o $count

Para contar el número de documentos existen varias maneras de realizar el procedimiento. La más sencilla es realizar un .count() después de un .find()

```javascript
mu.conn.then((client) => {
    client.db("Population")
      .collection("cities")
      .count()
      .toArray((err, data) => {
        console.log(data);
    }).finally(() => client.close());
});

```

Sin embargo, en sharded clusters ([https://docs.mongodb.com/manual/sharding/](https://docs.mongodb.com/manual/sharding/)), no es posible realizar un .count(), por lo cual se recomienda utilizar el $count del aggregation framework.
Utilizando $count tenemos (también es posible utilizar un $group por _id: null y contar los documentos con $sum)

```javascript
mu.conn.then((client) => {
    client.db("Population")
      .collection("cities")
      .aggregate([
{ $match: { name: ‘Envigado’ } },
{ $count: ‘count’ },
])	
      .toArray((err, data) => {
        console.log(data);
    }).finally(() => client.close());
});
```

## distinct()

Para obtener documentos distintos después de un find se utiliza .distinct()

```javascript
mu.conn.then((client) => {
    client.db("Population")
      .collection("cities")
      .distinct()
      .toArray((err, data) => {
        console.log(data);
    }).finally(() => client.close());
});
```

Sin embargo, en sharded clusters puede retornar orphaned documents. Por lo cual en este caso utilizamos $group para realizar un .distinct()

```javascript
mu.conn.then((client) => {
    client.db("Population")
      .collection("cities")
      .aggregate([
{
    $group : {
       {_id : null, uniqueValues: { $addToSet: “$description”} }  ,
    }
  },

 } }  
])	
      .toArray((err, data) => {
        console.log(data);
    }).finally(() => client.close());
});
```

En este caso utilizamos creamos un arreglo uniqueValues y agregamos cada valor al arreglo. **$addToSet** añade un valor a un arreglo a menos de que este ya esté repetido, en este caso si la descripción de la ciudad ya está añadida no se añade al arreglo **uniqueValues**.

## $lookup

Este pipeline permite realizer los joins que conocemos en bases de datos SQL

```javascript
mu.conn.then((client) => {
    client.db("Population")
      .collection("cities")
      .aggregate([
{
    $lookup: {
	from: ‘departments’, //colección de donde vamos a buscar los documentos que coincidan
	localField: ‘deparment’, //como se llama nuestro campo en la colección actual
	foreignField: ‘_id’, //como se llama el campo en la colección donde vamos a buscar
	as: ‘dep’  //el nombre que va a tener en el resultado, si es el mismo que el localField se sobreescribe el campo por el resultado
}	
  },

 } }  
])	
      .toArray((err, data) => {
        console.log(data);
    }).finally(() => client.close());
});
```

Es necesario realizar un **$group** después de un lookup cuando nuestro **foreignField** es de tipo **arreglo de ObjectIds (o arreglo en general)**

## $project

Sirve para que cuando realizamos una búsqueda completa solo nos retorne ciertos campos, así evitamos vulnerabilidades en seguridad y tener elementos innecesarios

```javascript
mu.conn.then((client) => {
    client.db("Population")
      .collection("cities")
      .aggregate([
{ $match: { name: ‘Envigado’ } },
{ $project: { description: 1, population:  0 } }
])	
      .toArray((err, data) => {
        console.log(data);
    }).finally(() => client.close());
});
```

En este caso solo recibimos la descripción y decimos que no vamos a recibir el número de habitantes

## $unwind

Funciona como un foreach de los elementos de un arreglo y podemos realizar consultas o modificaciones de estos arreglos

```javascript
mu.conn.then((client) => {
    client.db("Population")
      .collection("departments")
      .aggregate([
{ $unwind: { ‘$cities ’ }  },
{ $project: { ‘cities.population’: { $add: [ 1 ] } } }
])	
      .toArray((err, data) => {
        console.log(data);
    }).finally(() => client.close());
```


El código suma uno a los habitantes de todas las ciudades y lo muestra.


## $sort

Realiza un ordenamiento según un campo específico


``` javascript
mu.conn.then((client) => {
    client.db("Population")
      .collection("cities")
      .aggregate([
{
{ $sort: {  createdAt: -1 }}
}
])	
      .toArray((err, data) => {
        console.log(data);
    }).finally(() => client.close());
});
```

En este caso organizamos las ciudades en orden descendente según su fecha de creación.

# Documentación adicional

Esta wiki fue una breve introducción de lo que se puede realizar con el driver de MongoDB para Node.js. Para conocer la documentación oficial visite https://docs.mongodb.com/drivers/node/.