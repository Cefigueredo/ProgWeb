Bootstrap es el framework más popular para la construcción de sitios web responsives, es decir que se pueden adaptar a cualquier dispositivo (desktop, tablet, mobile). Bootstrap incluye elementos visuales preconstruidos para la creación de formularios, botones, tablas, navegación, entre otros, así como plugins de JavaScript para la creación de páginas web dinámicas.

La versión estable de Bootstrap es la 4.2.1. Puede ser descargada directamente de la [página oficial](https://getbootstrap.com/) para ser usada como un recurso local del proyecto web o accedida mediante un CDN (Content Distribution Network). Para cualquiera de las dos opciones, a continuación se muestra un template de base para el uso del framework en el proyecto web.

```html
  <!doctype html>
  <html lang="en">
    <head>
      <!-- Required meta tags -->
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

      <!-- Bootstrap CSS -->
      <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css" integrity="sha384-GJzZqFGwb1QTTN6wy59ffF1BuGJpLSa9DkKMp0DgiMDm4iYMj70gZWKYbI706tWS" crossorigin="anonymous">

      <title>Hello, world!</title>
    </head>
    <body>
      <h1>Hello, world!</h1>

      <!-- Optional JavaScript -->
      <!-- jQuery first, then Popper.js, then Bootstrap JS -->
      <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
      <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.6/umd/popper.min.js" integrity="sha384-wHAiFfRlMFy6i5SRaxvfOCifBUQy1xHdJ/yoi7FRNXMRBu5WHdZYu1hA6ZOblgut" crossorigin="anonymous"></script>
      <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/js/bootstrap.min.js" integrity="sha384-B0UglyR+jN6CkvvICOB2joaf5I4l3gm9GU6Hc1og6Ls7i6U/mkkaduKaBhlAXv9k" crossorigin="anonymous"></script>
    </body>
  </html>
```
|[Ver en Stackblitz](https://stackblitz.com/edit/bootstrap-basic-template?embed=1&file=index.html)|
|--|

Este tutorial se divide en varias secciones en las cuales se describen y ejemplifican algunos de los elementos más importantes de Bootstrap. Al final de cada sección se plantea un pequeño reto el cuál deberá ser resuelto a partir de lo aprendido hasta el momento. Las secciones son:

* [Layout](%5BBootstrap%5D-Layout)
* [Content](%5BBootstrap%5D-Content)
* [Components](%5BBootstrap%5D-Components)

Toda la documentación del framework puede ser encontrada en la [página oficial](https://getbootstrap.com/docs/4.2/getting-started/introduction/).