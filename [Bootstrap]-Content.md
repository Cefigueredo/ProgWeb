Otra ventaja del framework, además de su facilidad para construir páginas web responsives, es su gran cantidad de funcionalidades para hacer que la página web sea más amigable para el usuario.


## Reboot

Por defecto, Boostrap altera los estilos de muchos de los tags HTML que comúnmente se usan durante el desarrollo web. Es decir, tags de encabezados (`h1` a `h6`), tablas (`table`, `th`, `tr`, `td`), formularios (`form`, `fieldset`, `label`, `input`), entre otros, tendrán un aspecto visual diferente al de una página que no utilice Bootstrap.


## Typography

En cuanto al texto de la página web, un conjunto extenso de clases CSS habilitan al desarrollador para realizar cambios significativos en el aspecto visual, incluso entre dispositivos, como se mostró en la sección de Layouts.

HTML establece 6 tipos de tags de encabezado. Mediante un conjunto de clases con el mismo nombre del tag, el desarrollador puede aplicar los estilos que Bootstrap establece para los diferentes encabezados a un tag diferente (`p`, por ejemplo):

```html
  <p class="h1">h1. Bootstrap heading</p>
    <p class="h2">h2. Bootstrap heading</p>
    <p class="h3">h3. Bootstrap heading</p>
    <p class="h4">h4. Bootstrap heading</p>
    <p class="h5">h5. Bootstrap heading</p>
  <p class="h6">h6. Bootstrap heading</p>
```
|[Ver en Stackblitz](https://stackblitz.com/edit/bootstrap-headding-classes?embed=1&file=index.html)|
|--|

Para que el texto se muestre tachado, subrayado, en negrita o cursiva, se ofrece un nuevo conjunto de tags con estilos predefinidos. Elementos estándar de HTML5 como `b` e `i`, pueden ser usados conjuntamente.

```html
  <p>You can use the mark tag to <mark>highlight</mark> text.</p>
  <p><del>This line of text is meant to be treated as deleted text.</del></p>
  <p><s>This line of text is meant to be treated as no longer accurate.</s></p>
  <p><ins>This line of text is meant to be treated as an addition to the document.</ins></p>
  <p><u>This line of text will render as underlined</u></p>
  <p><small>This line of text is meant to be treated as fine print.</small></p>
  <p><strong>This line rendered as bold text.</strong></p>
  <p><em>This line rendered as italicized text.</em></p>
```
|[Ver en Stackblitz](https://stackblitz.com/edit/bootstrap-inline-text?embed=1&file=index.html)|
|--|


## Tables

Como se mencionó al inicio de la sección, Bootstrap redefine los estilos de las tablas, así como los de otros elementos. Adicionalmente, el desarrollador puede especificar clases adicionales que extienden el diseño y funcionalidad de la tabla. Algunos ejemplos son: 

- Invertir los colores de la tabla `.table-dark`
- Intercalar los colores de las filas `table-striped`
- Tabla con borde `table-bordered`
- Highlighting sobre la fila en la que se detecta el cursor del usuario `table-hover`

Así mismo, se pueden combinar todas estas clases de acuerdo a las necesidades del desarrollador.

```html
  <table class="table table-hover table-striped table-dark">
  <thead class="thead-light">
    <tr>
      <th scope="col">#</th>
      <th scope="col">First</th>
      <th scope="col">Last</th>
      <th scope="col">Handle</th>
    </tr>
    </thead>
    <tbody>
      <tr>
        <th scope="row">1</th>
        <td>Mark</td>
        <td>Otto</td>
        <td>@mdo</td>
      </tr>
      <tr>
        <th scope="row">2</th>
        <td>Jacob</td>
        <td>Thornton</td>
        <td>@fat</td>
      </tr>
      <tr>
        <th scope="row">3</th>
        <td colspan="2">Larry the Bird</td>
        <td>@twitter</td>
      </tr>
    </tbody>
  </table>
```
|[Ver en Stackblitz](https://stackblitz.com/edit/bootstrap-tables?embed=1&file=index.html)|
|--|


## Images

Como con cualquier otro elemento de contenido, Bootstrap proporciona mecanismos para hacer que las imágenes sean responsives. Esto se logra incluyendo la clase `img-fluid`.

```html
  <img class="img-fluid" src="...">
```
|[Ver en Stackblitz](https://stackblitz.com/edit/bootstrap-image-responsive?embed=1&file=index.html)|
|--|

Toda la documentación sobre Content puede ser encontrada en la [página oficial](https://getbootstrap.com/docs/4.2/content/).


## Reto

Extienda la clase `table-hover` para que el color gris que se usa por defecto para el highlighting cambie a rojo, verde o azul dependiendo del mamaño del dispositivo. 