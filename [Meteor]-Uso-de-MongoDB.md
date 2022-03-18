Meteor proporciona un conjunto de funcionalidades de alto nivel para persistir información en bases de datos de tipo documental, en este caso MongoDB, la cual se encuentra por defecto integrada con la plataforma y no requiere de configuraciones adicionales para su uso desde React.

Lo primero que debe hacer es crear el archivo `imports/api/forecast.js`. Este archivo es el que se encargará de establecer la conexión con MongoDB.

```javascript
import { Mongo } from 'meteor/mongo';

export const Forecast = new Mongo.Collection('forecast');
```

Posteriormente, diríjase al archivo `server/main.js` y añada la siguiente línea.

```javascript
import Forecast from '../imports/api/forecast.js';
```

Al importar el archivo encargado de crear la conexión en el archivo principal del lado del servidor, se habilitará la creación e intercambio de información entre el cliente y la base de datos de forma transparente y sin requerir de código adicional.

Para poder usar esta funcionalidad debe agregar un nuevo componente a la aplicación mediante el siguiente comando:

```
meteor add react-meteor-data
```

Finalmente, debe modificar el archivo `imports/ui/WeekForecast.js` para que el componente de React consulte MongoDB y disponga la información en la interfaz de usuario.

```javascript
import React, { Component } from "react";
import { withTracker } from 'meteor/react-meteor-data';

import { Forecast } from '../api/forecast.js';
import DayForecast from "./DayForecast.js";
 
class WeekForecast extends Component {

  renderDays() {
    return this.props.forecast.map((d) => (
      <DayForecast key={d._id} report={d} />
    ));
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

export default withTracker(() => {
  return {
    forecast: Forecast.find({}).fetch(),
  };
})(WeekForecast);
```

Para ingresar documentos a la colección, Meteor tiene su propia consola para acceder a MongoDB. Abra una nueva consola de linea de comandos e ingrese:

```
meteor mongo
```

El comando para ingresar un nuevo documento a la colección es:

```
db.forecast.insert({ dayName: "Sun", minTemp: 20, maxTemp: 40, status : "Mostly sunny" });
``` 

Si todo va bien, a medida de que ingresa nuevos documentos por medio de la consola de MongoDB, podrá verlos automáticamente en la interfaz de usuario sin requerir de la actualización de la página.



### Reto

* Modifique la apariencia de la página para mostrar, no solo información textual sino también grafíca. Tome como ejemplo [este reporte del tiempo ](https://i.imgur.com/PwVzLOg.png) (tomado de http://www.fox5dc.com).

* Cambie el componente principal de la aplicación por uno denominado MonthForecast. Este componente deberá mostrar el pronóstico del tiempo mensual.

<!--
### Reto 
* Continue desarrollando el tutorial de Meteor dispuesto en la [página oficial](https://www.meteor.com/tutorials/react/forms-and-events) cubriendo los aspectos de creación, actualización y eliminación de documentos desde la interfaz de usuario.
* Extienda la aplicación con los aspectos de seguridad abordados durante el mismo tutorial. 
* Agregue elementos de interacción complementarios que brinden al usuario la posibilidad de compartir tareas con otros usuarios, incluir fecha y hora límite para el desempeño de la tarea, creación de subtareas con estado (nueva / en curso / finalizada) independiente de la tarea principal e involucre componentes geográficos como los ofrecidos por [Google Maps Platform](https://cloud.google.com/maps-platform/?hl=es) para la georeferenciación de las tareas.
* Cree una vista adicional que le permita a un usuario administrador realizar consultas por estado, fecha y hora límites de ejecución y proximidad geográfica.
-->