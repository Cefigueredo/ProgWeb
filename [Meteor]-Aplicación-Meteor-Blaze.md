Para comenzar, ejecute el siguiente comando el cual creará la estructura básica de una aplicación en Meteor:

```
meteor create myapp
```

Una vez creada, ingrese a la carpeta de la aplicación e inicie el servidor:

```
cd myapp

meteor
```

Podrá ingresar a la aplicación mediante la URL [http://localhost:3000](http://localhost:3000).

Borre todo el contenido de la carpeta _client_ y cree un archivo `main.html` con el siguiente contenido:

```html
<head>
    <title>My first app in meteor</title>
</head>
<body>
    <h1>Hello world!</h1>
</body>
```

Dentro de la misma carpeta _client_ cree el archivo `main.js` con el siguiente contenido:

```javascript
import './main.html';
```
### Plantillas

Meteor usa Blaze como motor de plantillas. Las plantillas se escriben usando el lenguaje Spacebars. Modifique el archivo `main.html` para incluir una nueva plantilla.

```html
<head>
    <title>My first app in meteor</title>
</head>
<body>
    <h1>Using templates</h1>
    {{> profile}}
</body>

<template name="profile">
    <p>Name: Juan Rodríguez</p>
    <p>Age: 25</p>
</template>
```

En el fragmento anterior se está creando una plantilla denominada _profile_, la cual se incluye en el cuerpo de la página principal.

### Pasando datos a las plantillas

En el siguiente ejemplo, el nombre y la edad para el perfil se van a pasar como parámetros a la plantilla al momento de su inclusión:

```html
<head>
    <title>My first app in meteor</title>
</head>
<body>
    <h1>Using templates</h1>
    {{> profile name="Juan Rodríguez" age=25}}
</body>

<template name="profile">
    <p>Name: {{name}}</p>
    <p>Age: {{age}}</p>
</template>
```

### Usando condicionales en las plantillas

En el siguiente ejemplo, solo se mostrará el párrafo correspondiente a la edad, si ese parámetro fue pasado a la plantilla. Compruebe esto quitando el parámetro _age_ del llamado a la plantilla.

```html
<head>
    <title>My first app in meteor</title>
</head>
<body>
    <h1>Using templates</h1>
    {{> profile name="Juan Rodríguez" age=25}}
</body>

<template name="profile">
    <p>Name: {{name}}</p>
    {{#if age}}
    <p>Age: {{age}}</p>
    {{/if}}
</template>
```

### Usando helpers

Cada plantilla puede tener funciones (denominadas _helpers_). Existen dos funciones predefinidas que son llamadas cuando la plantilla es creada y renderizada.

Para usar los helpers predefinidos de una plantilla, edite el archivo `main.js` así y verifique la consola del navegador.

```javascript
import './main.html';

Template.profile.created = ()=>{
  console.log("Created the profile template");
}

Template.profile.rendered = ()=>{
  console.log("Rendered the profile template");
}
```
Ahora puede agregar un nuevo helper que se encagará de retornar un string.

```javascript

/*otros elementos del archivo*/

Template.profile.helpers({
    exampleHelper: () => {
        return "string returned by exampleHelper";
    }
});
```

Para usar el helper, modifique el archivo `main.html` así:

```html
<head>
    <title>My first app in meteor</title>
</head>
<body>
    <h1>Using templates</h1>
    {{> profile name="Juan Rodríguez" age=25}}
</body>

<template name="profile">
    {{exampleHelper}}
</template>
```


### Iterando sobre un array

En el siguiente ejemplo se usará un helper que retorna un array. Usando el block helper {{#each}} se hará una iteración sobre el array para mostrar los datos. Para ello, modifique el archivo `main.js` así:

```javascript
Template.profile.helpers({
    /*other helpers*/
    exampleHelper: () => {
        return "string returned by exampleHelper";
    },

    profileList: ()=>{
      return [
            {
                name: "Juan Rodríguez", age: 25
            },
            {
                name: "María Gómez", age: 30
            },
            {
                name: "Esteban Martínez", age: 15
            },
            {
                name: "Luisa Sánchez", age: 19
            }
        ] 
    }
});

```
Ahora modifique el arhivo `main.html` así:

```html
<head>
    <title>My first app in meteor</title>
</head>

<body>
    <h1>Using templates</h1>
    {{> profile}}
</body>

<template name="profile">
    {{#each profileList}}
    <p>Name: {{name}}</p>
    <p>Name: {{age}}</p>
    {{/each}}
</template>
```

### Pasar datos a un helper

El siguiente ejemplo ilustra cómo pasar datos a un helper. Modifique el arhcivo `main.js` así:

```javascript
Template.profile.helpers({
    /*other helpers*/

  passingData: (myString1, myString2) => {
    console.log(`These are the strings ${myString1} ${myString2}`);
  }
}
```

Ahora modifique el archivo `main.html` así:

```html
<head>
    <title>My first app in meteor</title>
</head>

<body>
    <h1>Using templates</h1>
    {{> profile}}
</body>

<template name="profile">
   {{passingData "string 1" "string2"}}
</template>
```

### Manejando eventos

Ahora se va a agregar un evento a una plantilla. En este ejemplo se incluye un botón que mostrará un número aleatorio cada vez que se le hace click. Para esto necesitamos agregar el módulo _session_ al proyecto. En la consola ejecute: `meteor add session`.

Modifique el arhcivo `main.js` así:

```javascript
Template.profile.helpers({
  /* Other helpers */
  randomHelper: () => {
        return Session.get("randomNumber");
    }
});

Template.profile.events({
    'click button': (e, i) => {
        console.log("Button clicked");
        Session.set("randomNumber", Math.random(0, 99));
    }
});
```

Ahora modifique el archivo `main.html` así:

```html
<template name="profile">
    <button>Random</button>
    {{randomHelper}}
</template>
```

### Usando colecciones en Meteor

Para usar colecciones de Mongo en Meteor, en el directorio raiz se debe crear un archivo para que sea accesible tanto por el cliente como por el servidor. Llame a este archivo `collections.js`, e incluya el siguiente contenido:

```javascript
Profiles = new Mongo.Collection("profiles");

export default Profiles;
```

Modifique el archivo `/server/main.js` para almacenar un conjunto de perfiles en la colección definida previamente:

```javascript
import { Meteor } from 'meteor/meteor';
import Profiles from "../collections";

Meteor.startup(() => {
  if (Profiles.find().count() === 0) {
    console.log("There are no profiles");
    let dummyProfiles = [
      { name: "Mariela Garcia", age: 50 },
      { name: "Fabio Garcia", age: 40 },
      { name: "Gustavo Garcia", age: 60 },
      { name: "Elvia Garcia", age: 65 },
    ];

    dummyProfiles.forEach(e => {
      Profiles.insert(e);
    })
  }
});
```

Modifique el archivo `/client/main.js` para crear un _helper_ que se encargue de traer la colección:

```javascript
import Profiles from "../collections";

/*other imports and methods */

Template.profile.helpers({
    
    /*other helpers*/
    
    profilesCollection: () => {
        return Profiles.find({});
    }
});
```

Ahora modifique el archivo `/client/main.html` para crear mostar los datos de la colección.

```html
<head>
    <title>My first app in meteor</title>
</head>

<body>
    <h1>Using templates</h1>
    {{> profile}}
</body>

<template name="profile">
    {{#each profilesCollection}}
    {{name}} {{age}}
    {{/each}}
</template>
```


### Reto

Crear una aplicación en Meteor que permita registrar los eventos organizados por la Universidad.

De cada evento se requiere el nombre, una descripción, el responsable, una fecha de inicio, una fecha final y una ubicación.

Cuando se registra el evento este debe quedar almacenado en una colección de Mongo y el listado de eventos publicados debe actualizarce automáticamente.

Para esto requiere:

* Crear una nueva plantilla en la que debe ubicar el formulario
* El formulario tendrá seis cajas de texto para cada elemento del evento
* El formulario tendrá un botón para enviar los datos
* Crear un nuevo helper para la plantilla 

```javascript
'submit form': (event) => {

}
```

* Para evitar que la página se recargue totalmente cuando se envían los datos del formulario, incluir dentro del _helper_ la instrucción `event.preventDefault();`.
* Los datos del formulario se obtienen en la propiedad `event.target`. Por ejemplo, con el siguiente código se obtienen los datos de la caja de texto con el nombre _name_.

```javascript
const target = event.target;
const name = target.name.value;
```

* Para insertar los datos en la colección puede llamar al método `Profiles.insert()` pasando como parámetro el objeto para insertar.


