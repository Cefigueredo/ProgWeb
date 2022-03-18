Para crear una aplicación con react use el comando `npx create-react-app my-app`

Si usa VS Code, instalar la extensión Simple React Snippets (by Burke Holland)

## Componente en una constante

Borre todo el contenido de la carpeta `src`.

Cree un archivo `src/index.js` con el siguiente contenido:

```javascript
import React from "react";
import ReactDOM from "react-dom";

const component = <h1>Hola mundo</h1>;

ReactDOM.render(component, document.getElementById("root"));
```

## Componente como una clase

```javascript
import React, { Component } from "react";
import ReactDOM from "react-dom";

class MyComponent extends Component {
  render() {
    return <h1>MyComponent</h1>;
  }
}

ReactDOM.render(<MyComponent />, document.getElementById("root"));
```

## Componente como una clase en un achivo externo

Cree un archivo denominado `src/Tweet.js`:

```javascript
import React, { Component } from "react";

class Tweet extends Component {
  render() {
    return <div>Tweet</div>;
  }
}

export default Tweet;
```

Modifique el archivo `src/index.js` así:

```javascript
import React from "react";
import ReactDOM from "react-dom";

import Tweet from "./Tweet";

ReactDOM.render(<Tweet />, document.getElementById("root"));
```

## Usar el estado de un componente

```javascript
import React, { Component } from "react";

class Tweet extends Component {
  state = {
    author: "John Doe",
    content: "React is a JS library for UI",
    likes: 25
  };

  render() {
    return (
      <div>
        <h1>{this.state.author}</h1>
        <p>{this.state.content}</p>
        <span>{this.state.likes}</span>
        <button>Like</button>
      </div>
    );
  }
}

export default Tweet;
```

## Renderizado condicional

```javascript
import React, { Component } from "react";

class Tweet extends Component {
  state = {
    author: "John Doe",
    content: "React is a JS library for UI",
    likes: 0
  };

  renderLikes() {
    return this.state.likes === 0 ? "Give us a like" : this.state.likes;
  }

  render() {
    return (
      <div>
        <h1>{this.state.author}</h1>
        <p>{this.state.content}</p>
        <span>{this.renderLikes()}</span>
        <button>Like</button>
      </div>
    );
  }
}

export default Tweet;
```

## Render lists

```javascript
import React, { Component } from "react";

class Tweet extends Component {
  state = {
    author: "John Doe",
    content: "React is a JS library for UI",
    likes: 0,
    tags: ["tag", "tag2", "tag3"]
  };

  renderLikes() {
    return this.state.likes === 0 ? "Give us a like" : this.state.likes;
  }

  handleIncrement = () => {
    this.setState({ likes: this.state.likes + 1 });
  };

  render() {
    return (
      <div>
        <h1>{this.state.author}</h1>
        <p>{this.state.content}</p>
        <ul>
          {this.state.tags.map(t => (
            <li key={t}>{t}</li>
          ))}
        </ul>
        <span>{this.renderLikes()}</span>
        <button>Like</button>
      </div>
    );
  }
}

export default Tweet;
```

## Handle events

```javascript
import React, { Component } from "react";

class Tweet extends Component {
  state = {
    author: "John Doe",
    content: "React is a JS library for UI",
    likes: 0,
    tags: ["tag", "tag2", "tag3"]
  };

  renderLikes() {
    return this.state.likes === 0 ? "Give us a like" : this.state.likes;
  }

  handleIncrement = () => {
    this.setState({ likes: this.state.likes + 1 });
  };

  render() {
    return (
      <div>
        <h1>{this.state.author}</h1>
        <p>{this.state.content}</p>
        <ul>
          {this.state.tags.map(t => (
            <li key={t}>{t}</li>
          ))}
        </ul>
        <span>{this.renderLikes()}</span>
        <button onClick={this.handleIncrement}>Like</button>
      </div>
    );
  }
}

export default Tweet;
```

## Parent and Child components

Cree un nuevo componente en `src/Tweets` con el siguiente componente:

```javascript
import React, { Component } from "react";

import Tweet from "./Tweet";

class Tweets extends Component {
  render() {
    return (
      <div>
        <Tweet />
        <Tweet />
        <Tweet />
        <Tweet />
      </div>
    );
  }
}

export default Tweets;
```

Modifique el archivo `src/index.js` así:

```javascript
import React from "react";
import ReactDOM from "react-dom";

import Tweets from "./Tweets";

ReactDOM.render(<Tweets />, document.getElementById("root"));
```

## Renderizar varios hijos dependiendo del estado del componente

```javascript
import React, { Component } from "react";

import Tweet from "./Tweet";

class Tweets extends Component {
  state = {
    tweets: [
      {
        author: "John Doe",
        content: "React is a JS library for UI",
        likes: 0,
        tags: ["tag1", "tag2", "tag3"],
        id: 1
      },
      {
        author: "Annie Hall",
        content: "React is a JS library for UI",
        likes: 20,
        tags: ["tag1", "tag2", "tag3"],
        id: 2
      },
      {
        author: "Albert Roger",
        content: "React is a JS library for UI",
        likes: 25,
        tags: ["tag1", "tag2", "tag3"],
        id: 3
      }
    ]
  };

  render() {
    return (
      <div>
        {this.state.tweets.map(t => (
          <Tweet tweet={t} id={t.id} />
        ))}
      </div>
    );
  }
}

export default Tweets;
```

Modifique para el el componente `Tweet` tome como estado los parámetros (props) que se le pasan.

```javascript
import React, { Component } from "react";

class Tweet extends Component {
  state = this.props.tweet;

  renderLikes() {
    return this.state.likes === 0 ? "Give us a like" : this.state.likes;
  }

  handleIncrement = () => {
    this.setState({ likes: this.state.likes + 1 });
  };

  render() {
    return (
      <div>
        <h1>{this.state.author}</h1>
        <p>{this.state.content}</p>
        <ul>
          {this.state.tags.map(t => (
            <li>{t}</li>
          ))}
        </ul>
        <span>{this.renderLikes()}</span>
        <button onClick={this.handleIncrement}>Like</button>
      </div>
    );
  }
}

export default Tweet;
```

## Ejecutar un evento del componente padre

Modifique el componente `Tweets` así:

```javascript
import React, { Component } from "react";

import Tweet from "./Tweet";

class Tweets extends Component {
  state = {
    tweets: [
      {
        author: "John Doe",
        content: "React is a JS library for UI",
        likes: 0,
        tags: ["tag1", "tag2", "tag3"],
        id: 1
      },
      {
        author: "Annie Hall",
        content: "React is a JS library for UI",
        likes: 0,
        tags: ["tag1", "tag2", "tag3"],
        id: 2
      },
      {
        author: "Albert Roger",
        content: "React is a JS library for UI",
        likes: 0,
        tags: ["tag1", "tag2", "tag3"],
        id: 3
      }
    ]
  };

  handleDelete = id => {
    const tweets = this.state.tweets.filter(t => t.id !== id);
    this.setState({ tweets });
  };

  render() {
    return (
      <div>
        {this.state.tweets.map(t => (
          <Tweet tweet={t} key={t.id} onDelete={this.handleDelete} />
        ))}
      </div>
    );
  }
}

export default Tweets;
```

Modifique el archivo `Tweet` así:

```javascript
import React, { Component } from "react";

class Tweet extends Component {
  state = this.props.tweet;

  renderLikes() {
    return this.state.likes === 0
      ? "Give us a like"
      : this.state.likes + " likes";
  }

  handleIncrement = () => {
    this.setState({ likes: this.state.likes + 1 });
  };

  renderTags() {
    return this.state.tags.map(e => <li key={e}>{e}</li>);
  }

  render() {
    return (
      <div>
        <h1>{this.state.author}</h1>
        <p>{this.state.content}</p>
        <ul>
          <p>{this.renderTags()}</p>
        </ul>
        <span>{this.renderLikes()}</span>
        <button onClick={this.handleIncrement}>Like</button>
        <button onClick={() => this.props.onDelete(this.props.tweet.id)}>
          Delete
        </button>
      </div>
    );
  }
}

export default Tweet;
```
