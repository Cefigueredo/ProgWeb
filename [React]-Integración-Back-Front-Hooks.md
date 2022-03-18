## Configuración del back

- Instale Express generator con el comando `npm install express-generator`

- Ejecute el comando `express app-back`. Esto creará una nueva carpeta que contiene la estructura de un nuevo proyecto en express.

- Ingrese a la carpeta `app-back` y ejecute `npm install` para instalar las dependencias necesarias.

- Instale nodemon con el comando `npm install nodemon`

- Abra el archivo packcage.json y modifique el atributo `scripts` de la siguiente forma:

```json
"scripts": {
  "start": "nodemon ./bin/www"
},
```

- Con `nodemon` el servidor se actualizará de forma automática cada vez que haya un cambio en los archivos del proyecto.

- Abra al archivo `bin/www` y modifique la línea 15 para que el servidor escuche peticiones en el puerto 3001

```javascript
var port = normalizePort(process.env.PORT || '3001');
```
- Ejecute `npm start`. Esto iniciará un servidor web para el back. Todo está correcto si al ingresar a http://localhost:3001 aparece una página con el mensaje "Express - Welcome to Express".


## Configuración del front

- Estando dentro de la carpeta `app-back` ejectute el comando `npx create-react-app front`. Esto crea una nueva carpeta en la que se va a desarrollar el proyecto del front.

- Borre todo el contenido de la carpeta `front/src`.

- Cree dentro de `front/src` una nueva carpeta denominada `components`.

- Cree un nuevo archivo en `front/src/components` denominado `Job.js` e incluya el siguiente código:

```javascript
import React from 'react';

const Job = () => {
    return (
      <div>
        This is a job offer.
      </div>
    );
}
export default Job;

```

- Cree un nuevo archivo en `front/src` denominado `index.js` e incluya el siguiente código:

```javascript
import React from "react";
import ReactDOM from "react-dom";

import Job from "./components/Job";

ReactDOM.render(<Job/>, document.getElementById("root"));

```
- Ingrese a la carpeta front `cd front` y ejecute el comando `npm start`. Esto iniciará un nuevo servidor web que escucha peticiones en el puerto 3000. Se deberá abrir una nueva ventana del browser con la url `http://localhost:3000/` que mostrará el mensaje "This is a job offer".

- Hasta el momento se tiene un servidor que se está ejecutando en el puerto 3001 y un cliente se está ejecutando en el puerto 3000

## Integración front-back

- En la carpeta `front` ejecute el comando `npm run build`. Esto compilará el proyecto `front` y creará una nueva carpeta denominada `build`.

- Vaya al directorio raiz, abra el archivo `app.js` y modifique la línea 20 por `app.use(express.static(path.join(__dirname, 'front/build')));`. Con esta modificación, todas las páginas que están en este directorio serán servidas al cliente como páginas estáticas. 

- Si ahora ingresa en un navegador a la url `http://localhost:3001`, podrá ver la página "estática" constuida en React.

- Finalmente, ingrese a la carpeta `front`, modifique el archivo `package.json` e incluya esta nueva línea: `"proxy": "http://localhost:3001",`. Esto permitirá que todas las peticiones que se hagan en el cliente y que tengan la forma "/ruta", serán resueltas en el servidor que corre en el puerto 3001.

## Ajustando el componente Job

- Cada oferta de trabajo deberá indicar el nombre de la oferta, el nombre de la compañía, el salario ofertado y la ciudad de la oferta. Para ello se debe crear un objeto `state` en `Job.js`, con los siguientes atributos:

```javascript
state = {
  "name": "Asesor comercial de hipermercado",
  "company": "Schneider Electric", 
  "salary": "$4.5 a $5.5 millones",
  "city":  "Bogotá, Colombia"
}
```

- Incluya un método `renderOffer` para que se encargue de formatear los valores de cada uno de los atributos del objeto `state`.

```javascript
const renderOffer = () => {
  return (
    <div>
      <h2>{state.name}</h2>
      <h3>{state.company}</h3>
      <h4>{state.salary}</h4>
      <h5>{state.city}</h5>
    </div>
  );
}
```
- Modifique el return del componente para hacer el llamado al método `renderOffer`

```javascript
  return (
    <div>
      {renderOffer()}
    </div>
  )
```

Dentro de la carpeta front ejecute nuevamente `npm run build`, y en el navegador vaya a `http://locahost:3001`. 

De ahora en adelante, cada vez que haga un cambio en el front, debe ejecutar `npm run build`.

## Creando una lista de ofertas

- Cree en la carpeta `front/src/components` un archivo denominado `Jobs.js`, con el siguiente código:

```javascript
import React from 'react';
import Job from "./Job";

const JobsList = () => {

  state = { 
  	"offers": [
  	{
  	  "name": "Asesor comercial de hipermercado",
  	  "company": "Schneider Electric", 
  	  "salary": "$4.5 a $5.5 millones",
  	  "city": "Bogotá, Colombia"
    }, 
    {
      "name": "Desarrollador de software",
      "company": "Google Inc.", 
      "salary": "$20 a 25 millones",
      "city": "Palo Alto, CA, USA"
      },
    ]
  };

  return (
  	  <div>
  	    {state.offers.map( (e,i) => <Job key={i}/>)}
          </div>
  );
}
export default JobList;
```

- Modifique el archivo `front/src/index.js` así:

```javascript
import React from "react";
import ReactDOM from "react-dom";

import Jobs from "./components/Jobs";

ReactDOM.render(<Jobs/>, document.getElementById("root"));
```

- En este momento, se deberá estar mostrando en el front dos ofertas. Sin embargo ambas tienen la misma información. Lo ideal es que la información de las ofertas detalladas en el objeto `state` del archivo `Jobs.js` sea la que se muestre en el front. Para ello, modifique el método `render` en `Jobs.js` así:

```javascript
  return (
    <div>
	  {state.offers.map( (e,i) => <Job key={i} offer={e}/>)}
    </div>
  )
```

- De este modo, al componente `Job` se le está definiendo una propiedad denominada `offer`, que tiene como valor un objeto en el array `offers` de `state`.

- Modifique el objeto state en el archivo `Job.js` y el método `renderOffer`, así:

```javascript
import React, { useState } from "react";

const Job = (props) => {
const [offer] = useState({
    name: props.offer.name,
    company: props.offer.company,
    salary: props.offer.salary,
    city: props.offer.city,
  });

  const renderOffer = () => {
    return (
      <div>
        <h2>{offer.name}</h2>
        <h3>{offer.company}</h3>
        <h4>{offer.salary}</h4>
        <h5>{offer.city}</h5>
        <hr />
      </div>
    );
  };

  return <div>{renderOffer()}</div>;
}


}

```

- En este caso, los valores para cada uno de los atributos del objeto `state`, se traen de la propiedad `offer` (`props.offer`). 


## Configuración variables de entorno
- En el directorio raíz del proyecto ejectute : npm install dotenv
- Luego cree un archivo .env, en el directorio raíz, que contenga la siguiente linea:

   mongourl= URL de su base de datos

- Por último vaya a al archivo app.js he incluya la siguiente linea 

```javascript
require('dotenv').config()
```

## Conexión con una base de datos Mongo

- El objetivo es modificar el archivo `Jobs.js`, para traer la lista de ofertas desde una colección en una base de datos MongoDB. 

- Para esto cree la conexión con la base de datos, agregando en el back un archivo denominado `db/Mongolib.js` con el siguiente contenido:

```javascript
const MongoClient = require('mongodb').MongoClient;
const assert = require('assert');

const url = process.env.mongourl;

const dbName = 'job';

const client = new MongoClient(url, { useUnifiedTopology: true });

const getDatabase = (callback) => {
    client.connect(function (err) {
        assert.equal(null, err);
        console.log("Connected successfully to server");

        const db = client.db(dbName);

        callback(db, client);
    });
}

const findDocuments = function (db, callback) {
    const collection = db.collection('offers');
    collection.find({}).toArray(function (err, docs) {
        assert.equal(err, null);
        callback(docs);
    });
}

exports.getDatabase = getDatabase;
exports.findDocuments = findDocuments;
```

- Agrege la libería `mongodb` al back ejecutando el comando `npm install mongodb`.

- Modifique el back con el fin de tener una ruta de la forma `/offers` que se encargue de retornar al cliente un JSON con el listado de ofertas. Inicie creeando un nuevo archivo `routes/offers.js`, con el siguiente contenido:

```javascript
var express = require('express');
var router = express.Router();
const Mongolib = require("../db/Mongolib");

/* GET home page. */
router.get('/', function (req, res, next) {
    Mongolib.getDatabase(db => {
        Mongolib.findDocuments(db, docs => {
            res.send(docs);
        })
    })
});

module.exports = router;
```

Luego, modifique el archivo `app.js` e incluya estas dos líneas:

```javascript
var offersRouter = require("./routes/offers");

//

app.use('/offers', offersRouter);

```

- Ahora vaya al front y modifique el archivo `Jobs.js` así:

```javascript
import React, {useState, useEffect} from 'react';
import Job from "./Job";

export const Jobs = () => {

  const [state, setState] = useState({offers:[]});

  useEffect(() => {
    const url = "/offers";
    fetch(url)
      .then(res => {
        return res.json();
      }).then(offers => {
        setState({ offers })
      });
  },[])

    return (
      <div>
        {state.offers.map((e, i) => <Job key={i} offer={e} />)}
      </div>
    );
}
```

## Reto
Cree un formulario en el front para guardar nuevos Jobs. Puede seguir el tutorial de creación de formularios de la wiki [Ir al tutorial de formularios](https://github.com/isis3710-uniandes/tutoriales/wiki/%5BReact%5D-Formularios). En el back usted debe crear un servicio(endpoint) en express con la lógica pertinente para guardar la información del Job en Mongo.



