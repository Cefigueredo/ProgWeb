## WebSockets

En este ejercio se construirá un chat usando el concepto de WebSockets para establecer comunicación bidireccional entre cliente y servidor.

Instale express generator: `npm install -g express-generator`

Cree un nuevo proyecto de express `express --no-view --git chat`

Ingrese a la carpeta chat e instale las dependencias: `npm install`

Instale la librería ws: `npm install ws -save`.

En la raiz cree un nuevo archivo `wslib.js` con el siguiente código:

```javascript
const WebSocket = require("ws");

const clients = [];
const messages = [];

const wsConnection = (server) => {
  const wss = new WebSocket.Server({ server });

  wss.on("connection", (ws) => {
    clients.push(ws);
    sendMessages();

    ws.on("message", (message) => {
      messages.push(message);
      sendMessages();
    });
  });

  const sendMessages = () => {
    clients.forEach((client) => client.send(JSON.stringify(messages)));
  };
};

exports.wsConnection = wsConnection;
```

Modifique el archivo `bin/www` y agregue la referencia al nuevo archivo creado:

```javascript
const ws = require("../wslib");
```

En ese mismo archivo, después de haber definido el servidor, incluya esta nueva línea:

```javascript
var server = http.createServer(app);
//después de haber definido server, incluya esta nueva línea
ws.wsConnection(server);
```

Modifique el archivo `public/index.html` así:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1>Chat</h1>
    <div id="messages"></div>
    <form id="form">
      <input type="text" id="message" />
      <input type="submit" value="submit" />
    </form>
    <script src="./javascripts/script.js"></script>
  </body>
</html>
```

Cree un nuevo archivo `public/javascripts/script.js` con el siguiente contenido:

```javascript
const ws = new WebSocket("ws://localhost:3000");

ws.onmessage = (msg) => {
  renderMessages(JSON.parse(msg.data));
};

const renderMessages = (data) => {
  const html = data.map((item) => `<p>${item}</p>`).join(" ");
  document.getElementById("messages").innerHTML = html;
};

const handleSubmit = (evt) => {
  evt.preventDefault();
  const message = document.getElementById("message");
  ws.send(message.value);
  message.value = "";
};

const form = document.getElementById("form");
form.addEventListener("submit", handleSubmit);
```

Desde una consola, ejecute `npm start`. Abra en dos navegadores la URL `http://localhost:3000/`.

Verifique que al enviar un mensaje al chat este se propaga a la otra ventana del navegador. 
