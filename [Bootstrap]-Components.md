Bootstrap proporciona una importante cantidad de componentes visuales y funcionales (+24) para disminuir el tiempo y complejidad al desarrollar páginas web. Todos estos componentes ya incluyen *responsiveness* siguiendo los mismos principios abordados en las anteriores secciones y también son altamente personalizables. Algunos ejemplos de componentes son:

## [Buttons](https://getbootstrap.com/docs/4.2/components/buttons/)

Se pueden realizar variaciones a nivel de color, tamaño, estado y función. Su forma más básica de instanciación es aplicando la clase de interés al tag `button`.

```html
  <button type="button" class="btn btn-primary">Primary</button>
```


## [Modals](https://getbootstrap.com/docs/4.2/components/modal/)

Son ventanas emergentes que pueden ser mostradas ante la acción del usuario o la actualización del estado de la página web (por ejemplo, la respuesta de un servicio web). Se pueden configurar de múltiples formas: informativas, de validación de la acción del usuario o incluso pueden incluir elementos más complejos como formularios y llamados a recursos externos (por ejemplo, un video de YouTube). Como muchos otros componentes, los modales se pueden instanciar directamente desde HTML, haciendo uso de atributos específicos dispuestos por Bootstrap para ese fin, o mediante JavaScript.

```html
  <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">Mostrar modal usando data-toggle</button>
  <button type="button" class="btn btn-primary" onclick="launchModal()">Mostrar modal usando JavaScript</button>

  <!-- Modal -->
  <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">Modal</h5>
          <button type="button" class="close" data-dismiss="modal" aria-label="Close">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <div class="modal-body">
          Contenido del modal
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
          <button type="button" class="btn btn-primary">Save changes</button>
        </div>
      </div>
    </div>
  </div>
```
```javascript
  function launchModal(){
    $('#exampleModal').modal()
  }
```
|[Ver en Stackblitz](https://stackblitz.com/edit/bootstrap-modals?embed=1&file=index.html)|
|--|


## [Navigation Bars](https://getbootstrap.com/docs/4.2/components/navbar/)

Las barra de navegación es otro de los aportes importantes de Bootstrap al desarrollo web. Son altamente configurables y se adaptan a cualquier tamaño de dispositivo. Se pueden instanciar de forma vertical y horizontal, se pueden crear menús con varios niveles de complejidad y, adicionalmente, se pueden integrar elementos de formulario como una caja de búsqueda.

```html
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <a class="navbar-brand" href="#">Navbar</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNavDropdown">
      <ul class="navbar-nav">
        <li class="nav-item active">
          <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Features</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Pricing</a>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownMenuLink" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
            Dropdown link
          </a>
          <div class="dropdown-menu" aria-labelledby="navbarDropdownMenuLink">
            <a class="dropdown-item" href="#">Action</a>
            <a class="dropdown-item" href="#">Another action</a>
            <a class="dropdown-item" href="#">Something else here</a>
          </div>
        </li>
      </ul>

      <form class="form-inline my-2 my-lg-0">
        <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search">
        <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
      </form>
    </div>
  </nav>
```
|[Ver en Stackblitz](https://stackblitz.com/edit/bootstrap-navbars?embed=1&file=index.html)|
|--|

Toda la documentación sobre Components puede ser encontrada en la [página oficial](https://getbootstrap.com/docs/4.2/components/).


## Reto

Implemente un botón que le permita al usuario agregar una nueva fila a una tabla mediante un formulario dispuesto en un modal. 