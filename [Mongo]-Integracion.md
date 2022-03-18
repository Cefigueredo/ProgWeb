### 
Después de crear una aplicación con express, instale mongo

`npm install mongod --save`

Descargue un json de ejemplo

`wget https://gist.githubusercontent.com/josejbocanegra/8490b48961a69dcd2bfd8a360256d0db/raw/34ff30dbc32392a69eb0e08453a3fc975a3890f0/series.json`

Importe la colección a Mongo

`mongoimport --db seriesdb --collection series --file series.json --jsonArray`

Cree un archivo de configuración `lib/mongoUtils` con el siguiente contenido:

```javascript
const { MongoClient } = require("mongodb");

const uri = "mongodb://localhost:27017/";
const conn = MongoClient.connect(uri, { useUnifiedTopology: true });

module.exports = conn;
```

Cree un controlador `controller/serie.js` con el siguiente contenido:

```
const mu = require("../lib/mongoUtils");

const getSeries = (callback) => {
  mu.then((client) => {
    client
      .db("seriesdb")
      .collection("series")
      .find({})
      .toArray((err, data) => {
        callback(data);
      });
  });
};

const addSerie = (serie) => {
  mu.then((client) => {
    client.db("seriesdb").collection("series").insertOne(serie);
  });
};

const serie = { getSeries, addSerie };

module.exports = serie;
```