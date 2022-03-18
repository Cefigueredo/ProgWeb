
# Objetivos
* Identificar las principales características de la herramienta Figma para el diseño de prototipos de interfaces de usuario.
* Diseñar un prototipo para versión móvil mediante la reutilización de componentes.


## Figma
Figma es un editor de gráficos vectorial y una herramienta de generación de prototipos, principalmente basada en la web, con características off-line adicionales habilitadas por aplicaciones de escritorio en macOS y Windows.

Dentro de sus principales características encontramos:
1. Permite trabajar colaborativamente en un archivo. Esto soluciona algunos problemas que se dan en equipos de diseño, cuando dos diseñadores quieren trabajar a la vez en un solo archivo.
2. Disponible en la nube. No necesita instalación.
3. Al igual que Sketch permite la creación de componentes para la reutilización de elementos de diseño.


## Tutorial

El siguiente tutorial tiene como finalidad acercar al desarrollador web al uso básico de Figma para leer y comprender prototipos de diseño.

1. Cree una cuenta en Figma. [Enlace Figma](https://www.figma.com/) 
2. Realice una copia local del siguiente proyecto [Link Prototipo Frenchie](https://www.figma.com/file/ZJS9WanXUVNaa2jl0W0fI5/Figma-para-Frontends?node-id=1%3A2), para esto seleccione el icóno de Figma en la barra superior **-> Files -> Save local copy.** el cual descargará un archivo .fig.

![Descargar copia local](https://i.imgur.com/Y50k48ll.png)

3. Abra el archivo que acaba de descargar desde Figma usando la cuenta previamente creada. A este punto usted deberá tener una copia del proyecto Frencie asociado a su cuenta.
4. Es importante reconocer las distintas zonas de trabajo de Figma. Para esto realice una exploración de los siguientes elementos: 

**Menú principal:** Ofrece la mayoría de las funcionalidades de Figma, entre otras permite:

* Mover y escalar diseños
* Crear Frames y exportar zonas en nuestro diseño. El uso de marcos es casi obligatorio, los usaremos para crear áreas de trabajo o artboards y zonas “inteligentes” que le darán vida a nuestros diseños. 
* Crear formas de Figma (Rectángulos, Líneas, Fechas, Ellipse, Stars)
* Dibujar vectores 
* Herramientas de edición de texto
* Herramienta de mano. Permite movernos entre todo el canvas para hacer foco en zonas específicas del diseño. 
* Comentarios. Permite dejar comentarios en el prototipo. 

![menu principal](https://i.imgur.com/7uXjispl.png)


**Menú lateral izquierdo**: Este menú ofrece una estructura de árbol de todos los elementos del prototipo y las páginas asociadas al mismo. Cada vez que se agrega un nuevo elemento al prototipo este se verá reflejado en este menú. Como ejercicio, cree un nuevo elemento rectángulo (Menú principal) e identifique su posición en el prototipo general. 

En nuestro proyecto de ejemplo tenemos tres páginas. La primera es **UI** la cual tiene los diseños referentes a la interfaz final de usuario, la segunda es **Design System** la cual contiene todos los componentes y material reutilizable de los diseños y por último, la página Thumbnail hace referencia a la vista previa del proyecto.

![menu izquierdo](https://i.imgur.com/pGjlOYol.png)

**Menú lateral derecho**: Este menú ofrece una vista detallada del elemento seleccionado. De acuerdo al elemento, Figma ofrece distintas opciones de configuración. Dentro de las opciones de configuración más comunes tenemos:

* Diseño: Permite configurar el diseño del elemento, desde definir su tamaño, bordes, colores, efectos y tipografía hasta producir la exportación del elemento de diseño en varios formatos.
* Prototipo: Sin lugar a dudas, esta opción permite crear diseños navegables y muy cercanos al producto final. Esta opción permite generar eventos de click, cambio de página o scroll a los elementos de diseño.
* Inspeccionar: En esta opción la posibilidad de ver un CSS propuesto por Figma del elemento seleccionado y a su vez permite ver las distancias en pixeles del elemento seleccionado con los demás elementos circundantes. 

![menu derecho](https://i.imgur.com/qgLFnCul.png)

A manera de ejercicio, seleccione el botón "Hacer pedido" e identifique las distancias en pixeles con el menú principal del diseño, con los bordes derecho y superior y finalmente con la imágen "The Frenchie". La siguiente imagen muestra como deben aparecer las mediciones en pixeles de un elemento respecto a los otros.

![inpect](https://i.imgur.com/Pystm9Il.png)

5. Creación de componentes: En este apartado vamos a identificar los componentes definidos en el prototipo.

Los componentes son la agrupación de uno o varios elementos de la interfaz que pueden ser reutilizados en diferentes situaciones y pueden ser modificados automáticamente si se modifica el componente principal.

En la página **Design System** identifique todos los elementos reutilizables. Entre estos encontrará la tipografía, algunas imágenes y los componentes. Realice un cambio en el color del componente llamado **button/primary** observe como todos los elementos que son instancia del button/primary cambian también de color. La siguiente imagen muestra la página **Design System**

![design system](https://i.imgur.com/7SHhTP2l.png)

Para crear un componente en su diseño sólo basta con seleccionar todos los elementos que van a conformar el componente y pulsar el botón en la barra superior “Create Component”. Lo que se obtiene es un Componente principal, el cual usted puede copiar las veces que necesite y por convención se debe colocar en la página **Design System*.


### Desafío
1. Tendiendo en cuenta la vista de escritorio del proyecto base, proponga un diseño para dispositivos móviles y para tablets. Estos diseños deben contener toda la información que aparece en el vista de escritorio.















 
 