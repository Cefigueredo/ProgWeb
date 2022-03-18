En este tutorial se creará una aplicación para visualizar un reporte semanal del clima.

Para comenzar, ejecute el siguiente comando el cual creará la estructura básica de la aplicación:

```
meteor create WeatherApp
```

Una vez creada, ingrese a la carpeta de la aplicación e inicie el servidor:

```
cd WeatherApp

meteor
```

Podrá ingresar a la aplicación mediante la URL [http://localhost:3000](http://localhost:3000).

<!-- 
Por defecto, el comando de creación de aplicaciones no instala las dependencias requeridas para el uso del framework React dado que inicialmente Meteor se desarrollo para usar su propio motor de templates llamado Blaze. Usted puede optar por usar Blaze y React de forma simultána dentro de la misma aplicación. Sin embargo, en este tutorial no están cubiertos temas referentes al uso de Blaze. Para remover Blaze e indicar a Meteor el uso de templates HTML estáticos ejecute lo siguientes comandos:

```
meteor npm install --save react react-dom
meteor add static-html
```
-->

Por defecto, el comando de creación de aplicaciones no instala las dependencias requeridas para el uso del framework React; por lo tanto, debe ejecutar el siguiente comando:

```meteor npm install --save react react-dom```

Con Meteor usted puede optar por usar Blaze y React de forma simultánea dentro de la misma aplicación. Sin embargo, en este tutorial no están cubiertos temas referentes al uso de Blaze. Para remover Blaze e indicar a Meteor el uso de templates HTML estáticos ejecute los siguientes comandos:

```
meteor remove blaze-html-templates

meteor add static-html
```

Luego, elimine todo el contenido de la carpeta `client` y cree dos archivos (`main.html` y `main.js`) con el siguiente contenido:

* main.html

```html
<head>
  <title>Document</title>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
</head>
<body>
  <div id="root" class="container">
    
  </div>  
</body>
```

En este fragmento de código se le está indicando a la aplicación que debe crear un elemento `div` con el id `root` y que sobre este elemento se renderizará la aplicación.

* main.js

```javascript
import React from "react";
import { Meteor } from "meteor/meteor";
import { render } from "react-dom";
 
import WeekForecast from "../imports/ui/WeekForecast.js";
 
Meteor.startup(() => {
  render(<WeekForecast />, document.getElementById("root"));
});
```

Usted debe crear manualmente la carpeta `imports/ui/`, y allí incluir el archivo `WeekForecast.js` que contiene el siguiente fragmento de código:

```javascript
import React, { Component } from "react";

import DayForecast from "./DayForecast.js";
 
export default class WeekForecast extends Component {

  getDays() {
    return [
      { _id: 1, dayName: "Sun", minTemp: 20, maxTemp: 40, status: "Mostly sunny"},
      { _id: 2, dayName: "Mon", minTemp: 20, maxTemp: 42, status: "Nice day" },
      { _id: 3, dayName: "Tue", minTemp: 30, maxTemp: 46, status: "Spring like" },
      { _id: 4, dayName: "Wed", minTemp: 38, maxTemp: 52, status: "Rain showers" },
      { _id: 5, dayName: "Thu", minTemp: 42, maxTemp: 52, status: "Warm pm shower" },
      { _id: 6, dayName: "Fri", minTemp: 26, maxTemp: 40, status: "Brigth colder" },
      { _id: 7, dayName: "Sat", minTemp: 20, maxTemp: 38, status: "Chilly" }
    ];
  }

  renderDays(){
    return this.getDays().map(d => <DayForecast key={d._id} report={d} />);
  }

  render() {
    return (
      <div>
        <div className="row text-center bg-success">
          <div className="col-sm">
            7 Day Forecast        
          </div>
        </div>
        <div className="row">
          {this.renderDays()}
        </div>
      </div>
    );
  }
}
``` 

Como puede observar, la funcionalidad básica de la aplicación involucra listar al usuario el pronóstico semanal. Para ello, la aplicación React hace uso de un componente adicional llamado `DayForecast.js` el cuál es el encargado de construir el pronóstico diario partir de la información transmitida desde el componente principal.

** DayForecast.js

```javascript
import React, { Component } from 'react';
 
export default class DayForecast extends Component {
  render() {
    return (
      <div className="col-sm">
        <p>{this.props.report.dayName}</p>
        <p>{this.props.report.minTemp}</p>
        <p>{this.props.report.maxTemp}</p>
        <p>{this.props.report.status}</p>
      </div>
    );
  }
}
```

* [Uso de MongoDB](%5BMeteor%5D-Uso-de-MongoDB)


