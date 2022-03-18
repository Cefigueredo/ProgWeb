## Formulario

Cree un nuevo componente denominado `signup.js` con el siguiente contenido:

``` javascript
function Signup() {
  return (
    <div>
      <form>
        <div>
          <label htmlFor="email">Email</label>
          <input
            type="email"
            id="email"
            name="email"
            autoComplete="password"
          />
        </div>

        <div>
          <label htmlFor="password">Password</label>
          <input
            type="password"
            id="password"
            name="password"
            autoComplete="password"
          />
        </div>

        <div>
          <label htmlFor="repeat_password">Confirm password</label>
          <input
            type="password"
            id="repeat_password"
            name="repeat_password"
            autoComplete="password"
          />
        </div>
        <button type="submit">Register</button>
      </form>
    </div>
  );
}

export default Signup;
```

El anterior es un formulario básico de registro con tres campos y un botón. 

Cree un hook para manejar el evento submit del formulario. Para esto, cree el archivo `customHooks.js` con el siguiente contenido:

``` javascript

const useSignUpForm = () => {

  const handleSubmit = (event) => {
    event.preventDefault();
    console.log("Form submitted");
  };

  return { handleSubmit };
};

export default useSignUpForm;
```
Este hook tiene una función que se llama cuando se realice _submit_ al formulario. 

Use el hook en el formulario. Para esto modifique el formulario así:


``` javascript
import useSignUpForm from "./customHooks";

function Signup() {
  const { handleSubmit } = useSignUpForm();
  return (
    <div>
      <form onSubmit={handleSubmit}>
        <div>
          <label htmlFor="email">Email</label>
          <input
            type="email"
            id="email"
            name="email"
            autoComplete="password"
          />
        </div>

        <div>
          <label htmlFor="password">Password</label>
          <input
            type="password"
            id="password"
            name="password"
            autoComplete="password"
          />
        </div>

        <div>
          <label htmlFor="repeat_password">Confirm password</label>
          <input
            type="password"
            id="repeat_password"
            name="repeat_password"
            autoComplete="password"
          />
        </div>
        <button type="submit">Register</button>
      </form>
    </div>
  );
}

export default Signup;

```

Capture los valores del formulario. Para esto, modifique el hook así:

``` javascript
import { useState } from "react";

const useSignUpForm = (schema) => {
  const [inputs, setInputs] = useState({});

  const handleSubmit = (event) => {
    event.preventDefault();
    console.log("Form submitted");
  };

  const handleInputChange = (event) => {
    setInputs({ ...inputs, [event.target.name]: event.target.value });
  };

  return { handleSubmit, handleInputChange };
};

export default useSignUpForm;
```

En el hook anterior se incluye una función que maneja el evento onChange de los campos del formulario. Cada vez que se modifica un campo, el nuevo valor se guarda en un hook de estado en la variable inputs. 

Ahora, modifique el formulario así:

``` javascript
import useSignUpForm from "./customHooks";

function Signup() {
  const { handleSubmit, handleInputChange } = useSignUpForm();
  return (
    <div>
      <form onSubmit={handleSubmit}>
        <div>
          <label htmlFor="email">Email</label>
          <input
            type="email"
            id="email"
            name="email"
            onChange={handleInputChange}
            autoComplete="password"
          />
        </div>

        <div>
          <label htmlFor="password">Password</label>
          <input
            type="password"
            id="password"
            name="password"
            autoComplete="password"
            onChange={handleInputChange}
          />
        </div>

        <div>
          <label htmlFor="repeat_password">Confirm password</label>
          <input
            type="password"
            id="repeat_password"
            name="repeat_password"
            autoComplete="password"
            onChange={handleInputChange}
          />
        </div>
        <button type="submit">Register</button>
      </form>
    </div>
  );
}

export default Signup;
```

Ahora, valide los campos del formulario. Para esto incluya un esquema de validación con Joi y pase el esquema al hook, así:

``` javascript

import useSignUpForm from "./customHooks";

import * as Joi from "joi";

const schema = Joi.object({
  email: Joi.string().email({ tlds: { allow: false } }),
  password: Joi.string().pattern(new RegExp("^[a-zA-Z0-9]{3,30}$")),
  repeat_password: Joi.ref("password"),
}).with("password", "repeat_password");

function Signup() {
  const { handleSubmit, handleInputChange } = useSignUpForm(schema);
  return (
    <div>
      <form onSubmit={handleSubmit}>
        <div>
          <label htmlFor="email">Email</label>
          <input
            type="email"
            id="email"
            name="email"
            onChange={handleInputChange}
            autoComplete="password"
          />
        </div>

        <div>
          <label htmlFor="password">Password</label>
          <input
            type="password"
            id="password"
            name="password"
            autoComplete="password"
            onChange={handleInputChange}
          />
        </div>

        <div>
          <label htmlFor="repeat_password">Confirm password</label>
          <input
            type="password"
            id="repeat_password"
            name="repeat_password"
            autoComplete="password"
            onChange={handleInputChange}
          />
        </div>
        <button type="submit">Register</button>
      </form>
    </div>
  );
}

export default Signup;
```

Ahora, modifique el hook así:

```
import { useState } from "react";

const useSignUpForm = (schema) => {
  const [inputs, setInputs] = useState({});
  const [errors, setErrors] = useState("");

  const handleSubmit = (event) => {
    event.preventDefault();
    const { error } = validate();
    if (!error) {
      console.log("Form submitted");
    } else {
      console.log(error);
      setErrors(error);
    }
  };

  const handleInputChange = (event) => {
    setInputs({ ...inputs, [event.target.name]: event.target.value });
  };

  const validate = () => {
    return schema.validate(inputs);
  };

  return { handleSubmit, handleInputChange, errors };
};

export default useSignUpForm;
```

## Reto

Muestre los mensajes de error en el formulario.


