Los layouts en Bootstraps, los cuales determinan la forma en la que se organizaran los diferentes contenidos de la página web, se construyen a partir de varios elementos, entre ellos: Containers, Responsive Breakpoints y Grid System.

## Containers

Existen dos tipos de Containers en Bootstrap: `container` y `container-fluid`. Mientras que `container` ajusta la página a un ancho fijo, dependiendo de las dimensiones del dispositivo, `container-fluid` busca aprovechar todo el ancho de la pantalla. 

La forma en la que se define un `container` es la siguiente. El ancho fijo por dispositivo puede ser ajustado mediante los Responsive Breakpoints, explicados más adelante.

```html
  <div class="container">
    ...
  </div>
```

Para definir un `container-fluid` basta con cambiar la clase del `div`.

```html
  <div class="container-fliud">
    ...
  </div>
```


## Grid System

Bootstrap implementa un sistema de 12 columnas para ajustar los bloques de contenido de forma variable entre cada uno de los 5 tamaños de dispositivos considerados. Cada tamaño de dispositivo tiene un identificador único:

|                              | Extra Small |   Small  |  Medium  |   Large  | Extra large |
|:----------------------------:|:-----------:|:--------:|:--------:|:--------:|:-----------:|
| Tamaño del dispositivo       |    <576px    |   ≥576px  |   ≥768px  |   ≥992px  |    ≥1200px   |
|      Prefijo de la clase     |    .col-    | .col-sm- | .col-md- | .col-lg- |   .col-xl-  | 

El desarrollador decide la cantidad de columnas (entre 1 y 12) a ser asignadas a un bloque de contenido específico. Una vez superado este límite, Bootstrap automáticamente hará uso de la siguiente fila.

El siguiente fragmento código muestra el funcionamiento básico del sistema de 12 columnas. Luego de definir el contenedor, se puede definir una fila (clase `row`) que funciona como un contenedor a un segundo nivel. Posteriormente, se definen 3 bloques de contenido con la clase `col-md-4` los cuales deben ajustarse al total de la pantalla, en donde `md` indica que la directriz debe aplicarse a pantallas de tamaño medio o superior y `4` que cada bloque de contenido debe ocupar un total de 4 columnas. Dado que no se está definiendo ninguna clase complementaria, en pantallas de menor tamaño cada bloque de contenido ocupara un total de 12 columnas.

```html
  <div class="container">
    <div class="row">
      <div class="col-md-4">
        1 of 3
      </div>
      <div class="col-md-4">
        2 of 3
      </div>
      <div class="col-md-4">
        3 of 3
      </div>
    </div>
  </div>
```
|[Ver en Stackblitz](https://stackblitz.com/edit/bootstrap-grid-basics?embed=1&file=index.html)|
|--|


## Responsive Breakpoints

Una forma práctica de entender estos elementos es como hojas de estilos personalizadas para diferentes dispositivos, usando los mismos rangos en pixeles del Grid System.

```css
  // Extra small devices (portrait phones, less than 576px)
  // No media query for `xs` since this is the default in Bootstrap

  // Small devices (landscape phones, 576px and up)
  @media (min-width: 576px) { ... }

  // Medium devices (tablets, 768px and up)
  @media (min-width: 768px) { ... }

  // Large devices (desktops, 992px and up)
  @media (min-width: 992px) { ... }

  // Extra large devices (large desktops, 1200px and up)
  @media (min-width: 1200px) { ... }
```
|[Ver en Stackblitz](https://stackblitz.com/edit/bootstrap-responsive-breakpoints?embed=1&file=index.html)|
|--|

Toda la documentación sobre Layouts puede ser encontrada en la [página oficial](https://getbootstrap.com/docs/4.2/layout/overview/).


## Reto

Cree una grilla que se ajuste a todo el ancho de la pantalla y que esté en la capacidad de mostrar al menos dos columnas independientemente del tamaño del dispositivo. Personalice la clase `text-size` usando breakpoints para que se incremente el tamaño de la letra en 10 pixeles entre dispositivos.  