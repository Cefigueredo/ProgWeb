En este tutorial se creará una página web internacionalizabe usando el módulo [React Intl](https://formatjs.io/docs/getting-started/installation/).

Para comenzar [haga un _fork_ a este repositorio](https://github.com/isis3710-uniandes/i18n).

Clone su repositorio bifurcado, instale las dependencias con `npm install` y ejectute el proyecto con `npm start`.

El navegador mostrará una página con una tabla, donde las filas representan ofertas de trabajo.

Si observa en el código fuente de la aplicación, todas las cadenas de texto están directamente incluidas en los componentes de React y no hay formatos especiales para fechas o números.  

El proceso de internacionalización inicia con la instalación de la librería React Intl. Para ello, ejectute `npm install react-intl --save`. 

Defina como componente principal a `IntlProvider` el cual es provisto por React Intl. Por tanto, modifique el archivo `src/index.js` así:

```javascript
import React from "react";
import ReactDOM from "react-dom";
import {IntlProvider} from 'react-intl';

import JobsList from "./components/jobsList";

ReactDOM.render(
	<IntlProvider locale="en">
		<JobsList/>
	</IntlProvider>, document.getElementById("root")
);
```
Esto indica que se usará el formato de localización en inglés, que es el formato por defecto.

Lo primera cadena que se localizará será la que representa la fecha de publicación de la oferta. Modifique el archivo `src/components/job.js` para importar la librería `FormattedDate` así:

```import {FormattedDate} from 'react-intl';```

Luego modifique la celda de la tabla en la que se muestra la fecha:

```jsx
<td>
  <FormattedDate
    value={new Date(props.offer.date)}
    year='numeric'
    month='long'
    day='numeric'
    weekday='long'
  />
</td>
```

## Retos

1. Agregue a la tabla una nueva columna que represente el número de visitas a la oferta. Localice el formato de ese número para aparezcan los separadores de miles, por ejemplo `134` o `1,250`.

2. Incluya la palabra `million` en los valores que representan el salario.

## Localizando otras cadenas de texto

Hasta el momento, las demás cadenas, por ejemplo, los encabezados de la tabla están definidos directamente sobre los componentes. Sin embargo, lo ideal es que estos cambien de acuerdo con el idioma del navegador o con las preferencias del usuario. 

Para nuestro caso se van a soportar 2 idiomas: español e inglés.

Cree una carpeta denominada ```src/locales```. Incluya 2 archivos `en.json` y `es.json`. Este es el ejemplo del contenido del archivo `es.json`:

```json
{
	"Position": "Nombre del cargo",
	"Company": "Empresa",
	"Salary": "Salario",
	"City": "Ciudad",
	"PublicationDate": "Fecha de publicación",
	"Views": "No. de visitas"
}
```
Este archivo contiene las traducciones en español las cadenas que representan los encabezados de las columnas.

Modifique el archivo `index.js` así:

```javascript
import localeEsMessages from "./locales/es";
```

Cambie el formato de localización por defecto e incluye el atributo ```messages``` en el componente ```IntlProvider```:

```jsx
ReactDOM.render(
	<IntlProvider locale="es-ES" messages= {localeEsMessages}>
		<JobsList/>
	</IntlProvider>, document.getElementById("root")
);
```
Esto hará que el formato de la fecha cambie.

Para cambiar una cadena en particular use el componente `<FormattedMessage>` pasándole el parámetro id con la cadena requerida. 

En este ejemplo, se cambia el encabezado de la columna Position (archivo jobsList.js).

```jsx
<th scope="col">
    <FormattedMessage id="Position"/>
</th>
```

## Retos

1. Determine dinámicamente el idioma del navegador y cambie tanto los mensajes como la localización que se pasan como parámetros al componente IntlProvider.

2. Reemplaze todas las cadenas estáticas por componentes FormattedMessage.

3. Modifique la cadena million por millón y millones respectivamente cuando el idioma del navegador sea español   

4. Cambie el color de fondo del encabezado de la tabla (light para español y dark para inglés) 
