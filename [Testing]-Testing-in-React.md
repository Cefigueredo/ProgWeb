## Prueba de una función

Cree una nueva aplicacion con `npx create-react-app`

Dentro de la carpeta `src` cree una nueva carpeta denominada `components`.

Cree un nuevo archivo denominado `sum.js` con el siguiente contenido:

```javascript
const sum = (value1, value2) => value1 + value2;

export default sum;
```

Dentro de la carpeta `components` cree una nueva carpeta denominada `__tests__` y agregue un archivo denominado `sum.test.js` con el siguiente contenido:

```
import sum from "../sum";

describe("sum", function () {
  it("adds 1 + 2 to equal 3", function () {
    expect(sum(1, 2)).toBe(3);
  });
});
```
Para ejecutar la prueba corra el comando `npm test`.

En esta prueba se pueden identificar varios elementos:

* La suite de pruebas que se declara con el método _describe_.
* La espeficación de la prueba declarada con el método _it_.
* La expectativa, declarada con el método _expect_, que en este caso es que el llamado al método _sum_ con los parámetros 1 y 2 de como resultado 3.

## Prueba de un componente en React

Dentro de la carpeta `src/components` cree un nuevo archivo denominado `checkbox.js`. Este componente tendrá un cuadro de chequeo (chekbox) que cambiará su etiqueta según se encuentre activo o inactivo.

El componente tiene el siguiente contenido:

```javascript
import React, { useState } from "react";

function Checkbox(props) {
  const [status, setStatus] = useState({ isChecked: false });

  const onChange = () => {
    setStatus({ isChecked: !status.isChecked });
  };

  return (
    <div>
      <label>
        <input type="checkbox" checked={status.isChecked} onChange={onChange} />
        {status.isChecked ? props.labelActive : props.labelInactive}
      </label>
    </div>
  );
}

export default Checkbox;
```

Modifique el archivo `src/index.js` para que el nuevo componente sea el principal, así: 

```javascript
import React from "react";
import ReactDOM from "react-dom";
import Checkbox from "./components/checkbox";

ReactDOM.render(
  <Checkbox labelActive="Active" labelInactive="Inactive" />,
  document.getElementById("root")
);
```

Cree un nuevo archivo de pruebas denominado `__tests__/checkbox.test.js`.

En esa prueba vamos a testear, que cuando el componente se renderiza, el contenido del label, será por defecto, "Inactive". 

Nótese que antes de cada prueba (ver método `beforeEach`), se asocia el componente a un nuevo `div`. Luego de cada prueba (ver método `afterEach`) el componente se retira del DOM.

El método `act` se encarga de renderizar el componente.

```javascript
import React from "react";
import ReactDOM from "react-dom";
import { act } from "react-dom/test-utils";
import Checkbox from "../checkbox";

let container;

beforeEach(() => {
  container = document.createElement("div");
  document.body.appendChild(container);
  act(() => {
    ReactDOM.render(
      <Checkbox labelActive="Active" labelInactive="Inactive" />,
      container
    );
  });
});

afterEach(() => {
  document.body.removeChild(container);
  container = null;
});

describe("Testing Checkbox component", () => {
  it("Defaults to Inactive label", () => {
    const label = container.querySelector("label");
    expect(label.textContent).toBe("Inactive");
  });
});

```

Ejecute la prueba con `npm test`.

La siguiente prueba verifica que el checkbox esté desmarcado por defecto:

```
//...
it("Checkbox inactive by default", () => {
  const checkbox = container.querySelector("input");
  expect(checkbox.checked).toBe(false);
});
```

## Probar eventos

El siguiente código se encarga de probar que cuando se hace click en el label, el estado del checkbox y del texto del label también cambian:

```
it("Checkbox status and label changes when clicked", () => {
  const checkbox = container.querySelector("input");
  const label = container.querySelector("label");
  act(() => {
    checkbox.dispatchEvent(new MouseEvent("click", { bubbles: true }));
  });
  expect(label.textContent).toBe("Active");
  expect(checkbox.checked).toBe(true);
});
```

## Reto

Probar el comportamiento de un componente denominado Like. Este componente está conformado por: (i) un párrafo que muestra el número de likes de una publicación en el formato "Likes: 0"; (ii) un botón "Likes" que incrementa en 1 el número de likes; y (iii) un botón "Dislike" que decrementa en 1 el número de likes. 

Este es el código del componente:

```
import React, { Component } from "react";

class Like extends Component {
  state = {
    likes: 0,
  };

  handleIncrement = () => {
    this.setState({ likes: this.state.likes + 1 });
  };

  handleDecrement = () => {
    this.setState({ likes: this.state.likes - 1 });
  };

  render() {
    return (
      <div>
        <p>Likes: {this.state.likes}</p>
        <button id="increment" onClick={this.handleIncrement}>
          Like
        </button>
        <button id="decrement" onClick={this.handleDecrement}>
          {" "}
          Dislike
        </button>
      </div>
    );
  }
}

export default Like;
```

### Pruebas a desarrollar

Incluya, en una _suite_, las siguientes pruebas:

1. Que por defecto, el componente muestra en el párrafo el valor "Likes: 0".
2. Que cuando se hace clic en el botón `Like`, el número de likes se incremente en uno.
3. Que cuando se hace clic en el botón `Dislike` el número de likes se decrementa en uno.



