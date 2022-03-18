## Configuración del back

Inicie creando una aplicación con express: `express --no-view reactive`.

Ingrese a la carpeta de la aplicación e instale dependencias (`npm install`). 

En la carpeta de la aplicación ejectute `npx create-react-app front`.

En el package.json del back cambie `"start": "node ./bin/www"` por `"start": "nodemon ./bin/www"`.

En `bin/www` cambie el puerto donde la aplicación escucha peticiones por 3001.

Modifique el archivo app.js así:

```javascript
var express = require('express');
var path = require('path');
var cookieParser = require('cookie-parser');
var logger = require('morgan');

var moviesRouter = require('./routes/movies');

var app = express();

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'front/build')));

app.use('/api/movies', moviesRouter);

module.exports = app;
```

Borre los archivos de la carpeta `routes` y cree uno denominado `movies` con el siguiente contenido:

```
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
    res.send([{name: "Casablanca"}, {name: "Citizen Kane"}])
});

module.exports = router;
```

Inicie la aplicación con `npm start` y verifique (desde un navegador, por ejemplo) que haciendo una petición GET a http://localhost:3001/movies obtiene un json con un conjunto de películas.

 
## Configuración del front

En el package.json del front agrege este nuevo atributo: `"proxy": "http://localhost:3001"`.

Modifique el componente principal (`App.js`) así:

```
import React, { useEffect, useState } from 'react';

function App() {

  const [movies, setMovies] = useState([]);
  
  useEffect(()=>{

    const URL ="/api/movies"
    fetch(URL).then(res => res.json()).then(res => {
      setMovies(res);
    })
  }, []);

  return (
    <div>
      <h1>Movies List</h1>
      <ul>
        {movies.map(m=><li>{m.name}</li>)}
      </ul>
    </div>
  );
}

export default App;

```

Inicie el front y deberá ver el listado de películas.

## Conexión con la BD

Instale mongodb con `npm install mongodb`.

En el back, cree el archivo `lib/mongoUtils.js` con el siguiente contenido:

```javascript
const { MongoClient } = require("mongodb");

const uri = "mongodb://localhost:27017/";
const conn = MongoClient.connect(uri, { useUnifiedTopology: true });

module.exports = conn;
```

Cree un archivo `controller/movies.js` con el siguiente contenido:

```javascript
const mu = require("../lib/mongoUtils");

const getMovies = (callback) => {
  mu.then((client) => {
    client
      .db("reactive")
      .collection("movies")
      .find({})
      .toArray((err, data) => {
        callback(data);
      });
  });
};

const movie = { getMovies };

module.exports = movie;
```

Modifique el archivo `routes/movies.js` así:

```javascript
var express = require('express');
var router = express.Router();

const movie = require("../controller/movies");

/* GET home page. */
router.get('/', function(req, res, next) {
  movie.getMovies(data=>{
    res.send(data)
  })
  
});

module.exports = router;
```

Ingrese algunos documentos en la colección movies y verifique que aparecen en el front. 

## Agregar reactividad vía WebSockets

Modifique el archivo `controller/movies.js` así:

```javascript
const mu = require("../lib/mongoUtils");

const getMovies = (callback) => {
  mu.then((client) => {
    client
      .db("reactive")
      .collection("movies")
      .find({})
      .toArray((err, data) => {
        callback(data);
      });
  });
};

const notifyChanges = (callback) => {
  mu.then((client) => {
    const cursor = client
    .db("reactive")
      .collection("movies")
      .watch()
      .on("change", (change)=>{
        console.log("Collection changing")
        getMovies(data => {
          console.log(data)
          callback(JSON.stringify(data));
        })
      })
  })
}

const movie = { getMovies, notifyChanges};

module.exports = movie;
```

Instale ws con `npm install ws`.

Cree un archivo `lib/wsUtils.js` con el siguiente contenido:

```javascript
const WebSocket = require("ws");

const clients = [];

const wsUtils = () => {
  const wsu = {}

  wsu.setupWS = (server) => {
    const wss = new WebSocket.Server({ server });
  
    wss.on("connection", (ws) => {
      clients.push(ws);
    });
  };

  wsu.notifyAll = (data) => {
    clients.forEach(c => c.send(data))
  }

  return wsu;
}

module.exports = wsUtils();
```

En `bin/www` incluya las siguientes líneas:

```
//Al inicio del documento
const wsUtils = require("../lib/wsUtils");
const movie = require("../controller/movies")

//Luego de registrar el servidor
var server = http.createServer(app);
wsUtils.setupWS(server)
movie.notifyChanges(wsUtils.notifyAll);
```

Finalmente, modifique el componente principal del front así:

```
import React, { useEffect, useState } from 'react';

function App() {

  const [movies, setMovies] = useState([]);

  const setupWS = () => {
    const wss = new WebSocket("ws://localhost:3001");
    wss.onopen = () => {
      wss.onmessage = (msg) => {
        console.log(msg)
        setMovies(JSON.parse(msg.data));
      }
    }
  }
  
  useEffect(()=>{
    setupWS();

    const URL ="/api/movies"
    fetch(URL).then(res => res.json()).then(res => {
      setMovies(res);
    })
  }, []);

  return (
    <div>
      <h1>Movies List</h1>
      <ul>
        {movies.map(m=><li>{m.name}</li>)}
      </ul>
    </div>
  );
}

export default App;
```

