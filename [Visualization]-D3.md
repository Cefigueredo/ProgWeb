## Get started

Creee una página web (`index.html`) con el siguiente contenido:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="https://d3js.org/d3.v5.min.js"></script>
    <title>Document</title>
</head>
<body>
    <div id="canvas">

    </div>
    <script src="app.js"></script>
</body>
</html>
```
En esta página todo el contenido será ubicado en el div con el id canvas.

Luego cree un archivo `app.js` en el que se incluirá el código para las visualizaciones.


### Selección de elementos

Con D3 se pueden seleccionar objetos del DOM de forma similar a como se hace con JQuery.

```javascript
const canvas = d3.select("#canvas");

const ul = canvas.append("ul");

ul.append("li").text("Item No. 1");
```

### Inclusión de elementos dinámicamente

```javascript
const canvas = d3.select("#canvas");

const data = [
    { name: "Juan", age: 3 },
    { name: "Fernanda", age: 16 },
    { name: "María", age: 7 },
    { name: "Sandra", age: 35 }
];

const ul = canvas.append("ul");

data.forEach(d => ul.append("li").text(d.name));
```

### Data binding

```javascript
const canvas = d3.select("#canvas");

const data = [
    { name: "Juan", age: 3 },
    { name: "Fernanda", age: 16 },
    { name: "María", age: 7 },
    { name: "Sandra", age: 35 }
];

const ul = canvas.append("ul");

const li = ul.selectAll("li").data(data);

li.enter()
    .append("li")
    .text(d => d.name);
```

### Crear figuras geométricas

```javascript
const canvas = d3.select("#canvas");

const svg = canvas.append("svg");

svg.attr("width", 800);
svg.attr("heigth", 800);

svg.append("rect")
    .attr("x", 10)
    .attr("y", 10)
    .attr("width", 100)
    .attr("height", 100)
    .style("fill", "steelblue");

```


## Barras con ejes

```javascript
const canvas = d3.select("#canvas");

const data = [
    {name: "Juan", age: 3},
    {name: "Orlando", age: 39},
    {name: "María", age: 7},
    {name: "Sandra", age: 35},
    {name: "Fernanda", age: 16},
    {name: "Maribel", age: 45},
    {name: "Sofía", age: 6}
];

const width = 700;
const height = 500;
const margin = { top:10, left:50, bottom: 40, right: 10};
const iwidth = width - margin.left - margin.right;
const iheight = height - margin.top -margin.bottom;

const svg = canvas.append("svg");
svg.attr("width", width);
svg.attr("height", height);

let g = svg.append("g").attr("transform", `translate(${margin.left},${margin.top})`);

const y = d3.scaleLinear() 
    .domain([0, 30])
    .range([iheight, 0]);

const x = d3.scaleBand()
.domain(data.map(d => d.name) ) 
.range([0, iwidth])
.padding(0.1); 

const bars = g.selectAll("rect").data(data);

bars.enter().append("rect")
.attr("class", "bar")
.style("fill", "steelblue")
.attr("x", d => x(d.name))
.attr("y", d => y(d.age))
.attr("height", d => iheight - y(d.age))
.attr("width", x.bandwidth())  

g.append("g")
.classed("x--axis", true)
.call(d3.axisBottom(x))
.attr("transform", `translate(0, ${iheight})`);  

g.append("g")
.classed("y--axis", true)
.call(d3.axisLeft(y));

```


### Reto No. 1

A partir de los datos de [este json](https://gist.githubusercontent.com/josejbocanegra/d3b9e9775ec3a646603f49bc8d3fb90f/raw/3a39300c2a2ff8644a52e22228e900251ec5880a/population.json), usar la librería d3 para crear una gráfica de barras horizontales, que en el eje x represente el número de refugiados y en el eje y el pais de procedencia.

Para facilitar la consulta de los datos puede usar la siguiente función:

```javascript
d3.json("url").then(data => {
    console.log(data);
});
```


### Reto No. 2

A partir de los datos de [este json](https://gist.githubusercontent.com/josejbocanegra/000e838b77c6ec8e5d5792229c1cdbd0/raw/83cd9161e28e308ef8c5363e217bad2b6166f21a/countries.json), usar la librería d3 para crear una gráfica con las siguientes características:

1. En el eje x se muestra el poder adquisitivo
2. En el eje y se muestra la expectiva de vida
3. Cada pais es representado como un círculo, y su radio es proporcional a la cantidad de habitantes.

Suba un archivo .js como respuesta a cada reto en la actividad de Sicua.