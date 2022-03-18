## Sequelize

[Sequelize](https://sequelize.org/) es un ORM de Node.js basado en promesas para Postgres, MySQL, MariaDB, SQLite y Microsoft SQL Server. 

En este tutorial integraremos Sequelize y Express.

Instale globalmente el paquete express-generator `npm install -g express-generator`.

Cree una nueva aplicación con express-generator: `express --no-view --git expressapi`.

Ingrese a la carpeta `expressapi` e instale las dependencias: `npm install`

Edite el archivo `package.json` y modifique la línea 6 así: `"start": "nodemon ./bin/www"`. (Asegurese de tener instalado globalmente nodemon `npm i -g nodemon`)

Inicie la aplicación con `npm start`.

Instale sequelize, sqlite3 y Joy

```
npm install --save sequelize
npm install --save sqlite3
npm install --save joi
```

Cree un nuevo archivo `lib/sequelize.js` para configurar la conexión a la base de datos: 

```javascript
const { Sequelize } = require("sequelize");

const sequelize = new Sequelize("database", "", "", {
  dialect: "sqlite",
  storage: "./database/database.sqlite",
});

sequelize.authenticate().then(() => {
  console.log("Authenticated!");
});

module.exports = sequelize;
```

Cree un nuevo archivo `models/client.js` para el modelo de `Client`.

```javascript
const { DataTypes, Model } = require("sequelize");
const sequelize = require("../lib/sequelize");

class Client extends Model {}

Client.init(
  {
    name: {
      type: DataTypes.STRING,
      allowNull: false,
    },
    email: {
      type: DataTypes.STRING,
      allowNull: false,
    },
  },
  {
    sequelize,
    timestamps: false,
    modelName: "Client",
  }
);

Client.sync();

module.exports = Client;

```

Modifique el archivo `app.js` así:

```javascript
var express = require("express");
var path = require("path");
var cookieParser = require("cookie-parser");
var logger = require("morgan");

var clientRouter = require("./routes/client");

var app = express();

app.use(logger("dev"));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, "public")));

app.use("/api/clients", clientRouter);

module.exports = app;
```
Borre el contenido de la carpeta `routes` y agregue un archivo `client.js` con el siguiente contenido:

```javascript
var express = require("express");
var router = express.Router();

const Joi = require("joi");
const Client = require("../models/client");

router.get("/", function (req, res, next) {
  Client.findAll().then((result) => {
    res.send(result);
  });
});

router.post("/", function (req, res, next) {
  const { error } = validateClient(req.body);

  if (error) {
    return res.status(400).send(error);
  }

  Client.create({ name: req.body.name, email: req.body.email }).then(
    (result) => {
      res.send(result);
    }
  );
});

router.get("/:id", (req, res) => {
  Client.findByPk(req.params.id).then((response) => {
    if (response === null)
      return res
        .status(404)
        .send("The client with the given id was not found.");
    res.send(response);
  });
});

router.post("/", (req, res) => {
  const { error } = validateClient(req.body);

  if (error) {
    return res.status(400).send(error);
  }

  Client.create({ name: req.body.name, email: req.body.email }).then(
    (result) => {
      res.send(result);
    }
  );
});

router.put("/:id", (req, res) => {
  const { error } = validateClient(req.body);

  if (error) {
    return res.status(400).send(error);
  }

  Client.update(req.body, { where: { id: req.params.id } }).then((response) => {
    if (response[0] !== 0) res.send({ message: "Client updated" });
    else res.status(404).send({ message: "Client was not found" });
  });
});

router.delete("/:id", (req, res) => {
  Client.destroy({
    where: {
      id: req.params.id,
    },
  }).then((response) => {
    if (response === 1) res.status(204).send();
    else res.status(404).send({ message: "Client was not found" });
  });
});

const validateClient = (client) => {
  const schema = Joi.object({
    name: Joi.string().min(3).required(),
    email: Joi.string().email(),
  });

  return schema.validate(client);
};

module.exports = router;
```
