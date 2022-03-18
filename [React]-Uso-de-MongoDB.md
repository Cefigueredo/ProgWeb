## Reto

Modifique el archivo `routes/offers.js` para que cuando el cliente haga una petición POST de la forma http://localhost:3001/jobOffer, el servidor cree una nueva oferta de trabajo en la BD.

```javascript
router.post('/jobOffer', function(req, res) {
  //Incluya acá la conexión a la BD, obtenga los parámetros, guarde la oferta en la BD y retorne un mensaje al cliente. 
});
```

Para completar este reto es importante crear un nuevo componente de React que se encargue de mostrar un formulario para ingresar las nuevas ofertas de trabajo.

Esta es una posible alternativa para construir el formulario en un componente denominado `CreateOffer`:

```javascript
render() {
  return (
    <div>
      <h1>Add a new offer</h1>
      <form onSubmit={this.handleSubmit}>
        <label for="name">Name:</label>
        <input id="name" type="text" name="name" onChange={this.handleChange} />
        <label for="company"> Company:</label>
        <input id="company" type="text" name="company" onChange={this.handleChange} />
        <label for="salary">Salary:</label>
        <input id="salary" type="text" name="salary" onChange={this.handleChange} />
        <label for="city">City:</label>
        <input id="city" type="text" name="city" onChange={this.handleChange} />
        <input type="submit" value="Submit" />
      </form>
    </div>
  );
}
```

A continuación se indica el código para los métodos `handleChange` y `handleSubmit`. 

```javascript
handleChange = (event) => {
  const target = event.target;
  const value = target.type === 'checkbox' ? target.checked : target.value;
  const name = target.name;

  this.setState({
      [name]: value
  });
}

handleSubmit = (event) => {
  fetch("/offers", {
      method: 'POST',
      headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json',
      },
      body: JSON.stringify(this.state)
  }).then(res => {
      return res.json();
  }).then(res => {
      console.log("Res", res);
      this.props.action();
  });

  event.preventDefault();
}
```

El método `handleChange` se encarga de ir almacenando en el objeto `State`, los valores de cada campo del formulario. 

El evento `handleSubmit` se encarga de enviar al endpoint `/offers` los valores capturados por el formulario y almacenados en el objeto `state`. Una vez se ha realizado el envío, se llama al método `this.props.action`, el cual es una propiedad del componente padre de `CreateOffer`, que en este caso es el componente `Jobs`. Por tanto se debe incluir en el archivo `Jobs.js`, en la línea donde se inserta el componente `CreateOffer` un atributo `action` con el siguiente valor: `<CreateOffer action={() => this.componentDidMount(this)} />`. De este modo, cada vez que se inserte una nueva oferta de trabajo desde el formulario de creación de ofertas, la lista se actualizará en consecuencia.




