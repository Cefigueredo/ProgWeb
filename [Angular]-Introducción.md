### Sobre el framework

Angular es una plataforma para desarrollar SPAs (single-page applications). Angular está escrito en Typescript.

El concepto fundamental de Angular (Building-Block) es el de componente. Un componente controla una zona de la pantalla llamada vista (view), esto es, muestra información y/o elementos de interacción para el usuario como botones, ménus, etc

Cada componente se define en una clase anotada/decorada con @Component y tiene asociado una vista o template en Html donde está la definición de los elementos que se van a desplegar. La clase del componente es quien se ocupa de mostrar información o de ejecutar comportamiento como consecuencia de las acciones del usuario. Entonces se establece una relación entre la clase del componente y el Html que lo va a mostrar en la pantalla.
Los componentes se "componen" para crear componentes más complejos. Los componentes se agrupan de manera lógica en módulos que van a permitir organizar la aplicación.

### Módulos y Componentes
Una aplicación Angular se organiza en un conjunto de módulos (@NgModules). Siempre habrá un módulo principal raíz y otros módulos que agrupan de manera lógica las funcionalidades de la aplicación.

Serán decisiones del diseñador de la aplicación definir cuántos módulos se crearán, qué responsabilidad tiene cada uno y cómo se relacionan los módulos entre sí.

Un módulo define (declara) uno o más componentes. Los componentes son los elementos básicos que definen una funcionalidad de la aplicación y con la que el usuario podrá interactuar. Un componente tiene una vista (html) y una lógica que permitirá ver, elegir y modificar información de la aplicación.

Los componentes utilizan servicios que proporcionan una funcionalidad específica que no está directamente relacionada con las vistas. Los servicios pueden inyectarse en los componentes como dependencias, haciendo el código más modular, reutilizable y eficiente.

### Relaciones entre los Módulos

Un módulo Angular declara componentes, exporta componentes y provee servicios. También puede importar otros módulos que contienen componentes y/o servicios que se necesitan para su funcionalidad.

### Librerías de Angular
Cuando construimos una aplicación web utilizando Angular, debemos importar una serie de librerías predefinidas. Estas librerías comienzan por @angular y las básicas son: '@angular/core', '@angular/platform-browser', '@angular/forms', '@angular/common/http'.

### Estructura de archivos de una aplicación Angular
En una aplicación Angular podemos encontrar archivo de Typescript (.ts), archivos Html (.html), archivos de estilos (.css) y archivos de configuración para distintos propósitos que típicamente son (.json).

Cuando se crea una aplicación Angular, en la carpeta src tendremos algunos de los siguientes archivos:
- src
  + app       // aquí van los módulos, componentes, servicios, pruebas
  index.html  // página principal
  main.ts     // declaración del módulo principal
  style.css   // estilos

En la carpeta app tenemos los módulos. El principal y su componente van en la raíz y la recomendación es tener una carpeta aparte por cada módulo adicional y dentro de esta una carpeta por cada componente:

- src
  - app 
    + moduloA
    + moduloB
    + ...
    app.module.ts           // módulo principal
    app.componente.ts       // componente principal del módulo principal
    app.component.spec.ts   // pruebas para el componente principal
    app.component.html      // template del componente principal
    app.components.css      // estilos del móduloprincipal

