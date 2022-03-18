## Animaciones

### Cambio de posición

```javascript
const svg = d3.select("#canvas").append("svg")
    .attr("width", 400)
    .attr("height", 200)
    .style("border-color", "black")
    .style("border-style", "solid")
    .style("border-width", "1px");

const rectangle = svg.append("rect")
    .attr("x", 50)
    .attr("y", 50)
    .attr("width", 50)
    .attr("height", 50);

d3.select("#start").on("click", function () {
    rectangle
        .transition()
        .attr("x", 250);
});
```

### Botón para resetear

```
d3.select("#reset").on("click", function () {
  rectangle.transition().attr("x", 50);
});
```

### Cambio de tamaño

```javascript
const svg = d3.select("#canvas").append("svg")
    .attr("width", 400)
    .attr("height", 200)
    .style("border-color", "black")
    .style("border-style", "solid")
    .style("border-width", "1px");

const rectangle = svg.append("rect")
    .attr("x", 50)
    .attr("y", 50)
    .attr("width", 50)
    .attr("height", 50);

d3.select("#start").on("click", function () {
    rectangle
        .transition()
        .attr("x", 250)
        .attr("width", 100) 
	    .attr("height", 100); 
});

d3.select("#reset").on("click", function () {
    rectangle
        .transition()
        .attr("x", 50);
});
```

### Cambio de opacidad

```javascript
const svg = d3.select("#canvas").append("svg")
    .attr("width", 400)
    .attr("height", 200)
    .style("border-color", "black")
    .style("border-style", "solid")
    .style("border-width", "1px");

const rectangle = svg.append("rect")
    .attr("x", 50)
    .attr("y", 50)
    .attr("width", 50)
    .attr("height", 50);

d3.select("#start").on("click", function () {
    rectangle
        .transition()
        .attr("x", 250)
        .attr("width", 100) 
	    .attr("height", 100) 
        .attr("opacity",0.5);
});

d3.select("#reset").on("click", function () {
    rectangle
        .transition()
        .attr("x", 50);
});
```

### Delay en el inicio de la transición

```javascript
const svg = d3.select("#canvas").append("svg")
    .attr("width", 400)
    .attr("height", 200)
    .style("border-color", "black")
    .style("border-style", "solid")
    .style("border-width", "1px");

const rectangle = svg.append("rect")
    .attr("x", 50)
    .attr("y", 50)
    .attr("width", 50)
    .attr("height", 50);

d3.select("#start").on("click", function () {
    rectangle
        .transition()
        .attr("x", 250)
        .attr("width", 100) 
	    .attr("height", 100) 
        .delay(500);
});

d3.select("#reset").on("click", function () {
    rectangle
        .transition()
        .attr("x", 50);
});
```

### Duración de la transición

```javascript
const svg = d3.select("#canvas").append("svg")
    .attr("width", 400)
    .attr("height", 200)
    .style("border-color", "black")
    .style("border-style", "solid")
    .style("border-width", "1px");

const rectangle = svg.append("rect")
    .attr("x", 50)
    .attr("y", 50)
    .attr("width", 50)
    .attr("height", 50);

d3.select("#start").on("click", function () {
    rectangle
        .transition()
        .attr("x", 250)
        .attr("width", 100) 
	    .attr("height", 100) 
        .delay(500)
        .duration(5000);
});

d3.select("#reset").on("click", function () {
    rectangle
        .transition()
        .attr("x", 50);
});
```

### Easing

```javascript
const svg = d3.select("#canvas").append("svg")
    .attr("width", 400)
    .attr("height", 200)
    .style("border-color", "black")
    .style("border-style", "solid")
    .style("border-width", "1px");

const rectangle = svg.append("rect")
    .attr("x", 50)
    .attr("y", 50)
    .attr("width", 50)
    .attr("height", 50);

d3.select("#start").on("click", function () {
    rectangle
        .transition()
        .attr("x", 250)
        .attr("width", 100) 
	    .attr("height", 100) 
        .delay(500)
        .duration(5000)
        .ease(d3.easeBounce);
});

d3.select("#reset").on("click", function () {
    rectangle
        .transition()
        .attr("x", 50);
});
```

### Doble transición

```javascript
const svg = d3.select("#canvas").append("svg")
    .attr("width", 400)
    .attr("height", 200)
    .style("border-color", "black")
    .style("border-style", "solid")
    .style("border-width", "1px");

const rectangle = svg.append("rect")
    .attr("x", 50)
    .attr("y", 50)
    .attr("width", 50)
    .attr("height", 50);

d3.select("#start").on("click", function () {
    rectangle
        .transition()
        .attr("x", 250)
        .attr("width", 100) 
	    .attr("height", 100) 
        .delay(500)
        .duration(5000)
        .ease(d3.easeBounce)
        .on("end",function() { 
		    d3.select(this)
		    	.transition() 
                .attr("x", 150) 
                .attr("width", 75) 
                .attr("height", 75) 
                .attr("opacity", 0.75) 
                .attr("fill", "blue") 
                .delay(500)  
                .duration(2500) 
                .ease(d3.easeBounce); 
		});
});

d3.select("#reset").on("click", function () {
    rectangle
        .transition()
        .attr("x", 50);
});
```

Reto No. 1: hacer una gráfica de barras, en la que el eje x represente a cada ciudad dentro del arreglo `data`, y la altura de la barra sea el dato `index2005`. Incluya un botón y una transición para que se tome como referencia el dato `index2006`. En la transición cambie el color de las barras.

```javascript
const data = [
    { name: "Medellín", index2005: 3, index2006: 33 },
    { name: "Cali", index2005: 39, index2006: 45 },
    { name: "Bogotá", index2005: 7, index2006: 31 },
    { name: "Pereira", index2005: 35, index2006: 36 },
    { name: "Bucaramanga", index2005: 16, index2006: 23 },
    { name: "Cúcuta", index2005: 45, index2006: 45 },
    { name: "Armenia", index2005: 6, index2006: 16 }
];
```
Reto No. 2: incluya la gráfica y la animación como un componente de React.

La visualización del componente la puede hacer de la siguiente forma:

```javascript
import React, { Component } from "react";
import * as d3 from "d3";

class Animation extends Component {
  componentDidMount() {
    const data = [2, 4, 2, 6, 8];
    this.drawChart(data);
  }

  drawChart(data) {
    const canvas = d3.select(this.refs.canvas);
  }

  render() {
    return <div ref="canvas"></div>;
  }
}

export default Animation;
```

Recuerde instalar la librería como dependencia del proyecto con el comando `npm install d3`