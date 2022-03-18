## Hooks en React

Los hooks en React son una aproximación para reducir la complejidad que implica el crear componentes como clases. 

Los dos hooks más útiles son _State_ y _Effect_, los cuales los abordaremos en las siguientes secciones.

## Componente como una función

Para usar los hooks, los componentes los escribiremos no como clases sino como funciones. 

Cree un nuevo proyecto, borre todo el contenido de la carpeta `src` y cree un archivo `src/app.js` con el siguiente contenido:

```javascript
import React, { useState } from "react";

function App() {
  let [author, setAuthor] = useState("John Doe");
  let [content, setContent] = useState("React is a JS library for UI");
  let [likes, setLikes] = useState(25);

  return (
    <div>
      <h1>Post</h1>
      <h2>Author: {author}</h2>
      <h2>Content: {content}</h2>
      <h2>Likes: {likes}</h2>
    </div>
  );
}

export default App;

```

Cree un archivo `src/index.js` con el siguiente contenido:

```javascript
import React from "react";
import ReactDOM from "react-dom";

import App from "./app";

ReactDOM.render(<App />, document.getElementById("root"));
```

En el ejemplo, a diferencia de los componentes basados en clases, no tenemos un objeto state, sino que usamos dos variables que son las que devuelven el hoot useState. La primera variable representa aquella en la que almacenaremos el estado, y la segunda es una función que se invoca para modificar el valor de ese estado. _setState_ recibe como parámetro el valor inicial del estado. 

Como pueden observar en el ejemplo, ya no es necesario el uso de operador `this.state` para obtener el valor de un estado; simplemente con referenciar a la variable tenemos acceso al valor del estado. 

## Renderizado condicional

```javascript
import React, { useState } from "react";

function App() {
  let [author, setAuthor] = useState("John Doe");
  let [content, setContent] = useState("React is a JS library for UI");
  let [likes, setLikes] = useState(25);

  let renderLikes = () => (likes === 0 ? "Give us a like" : likes);

  return (
    <div>
      <h1>Post</h1>
      <h2>Author: {author}</h2>
      <h2>Content: {content}</h2>
      <h2>Likes: {renderLikes()}</h2>
    </div>
  );
}

export default App;
```

## Handle events

```javascript

import React, { useState } from "react";

function App() {
  let [author, setAuthor] = useState("John Doe");
  let [content, setContent] = useState("React is a JS library for UI");
  let [likes, setLikes] = useState(0);
  let [tags, setTags] = useState(["tag1", "tag2", "tag3"]);

  let handleClick = () => {
    setLikes((likes = likes + 1));
  };

  let renderLikes = () => (likes === 0 ? "Give us a like" : likes);

  return (
    <div>
      <h1>Post</h1>
      <h2>Author: {author}</h2>
      <h2>Content: {content}</h2>
      <h2>Likes: {renderLikes()}</h2>
      <ul>
        {tags.map((item) => (
          <li>{item}</li>
        ))}
      </ul>
      <button onClick={handleClick}>Like</button>
    </div>
  );
}

export default App;
```

## Renderizar varios hijos dependiendo del estado del componente padre

Componente Tweets: `src/tweets.js`:

```javascript
import React, { useState } from "react";
import { Button, Form } from "react-bootstrap";

import Tweet from "./tweet";

function Tweets() {
  let [tweets, setTweets] = useState([
    {
      author: "John Doe",
      content: "React is a JS library for UI",
      likes: 0,
      tags: ["tag1", "tag2", "tag3"],
      id: 1,
    },
    {
      author: "Annie Hall",
      content: "React is a JS library for UI",
      likes: 20,
      tags: ["tag1", "tag2", "tag3"],
      id: 2,
    },
    {
      author: "Albert Roger",
      content: "React is a JS library for UI",
      likes: 25,
      tags: ["tag1", "tag2", "tag3"],
      id: 3,
    },
  ]);

  let renderTweets = () => {
    return tweets.map((item, i) => (
      <div>
        <Tweet key={i + 1} data={item} />
      </div>
    ));
  };

  return (
    <div className="p-4">
      {renderTweets()}
    </div>
  );
}

export default Tweets;
```

Componente Tweet: `src/tweet.js`:

```javascript
import React from "react";
import { Card } from "react-bootstrap";

function Tweet(props) {
  return (
    <Card style={{ width: "18rem" }}>
      <Card.Body>
        <Card.Title>{props.data.author}</Card.Title>
        <Card.Text>{props.data.content}</Card.Text>
      </Card.Body>
    </Card>
  );
}

export default Tweet;
