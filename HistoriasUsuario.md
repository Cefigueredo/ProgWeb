# Historias de Usuario
1. [Definición](#1Definicion)
2. [Criterios `INVEST `para revisar las HU](#2Criterios)
   * [Independiente](#21independiente)
   * [Negociable](#22Negociable)
   * [Valiosa](#23Valiosa) 
   * [Estimable](#24Estimable)
   * [Pequeña (del tamaño adecuado)](#25Tamanio)
   * [Que se pueda probar](#26Probar)
3. [Ejemplo](#3Ejemplo)
4. [Ejemplo Bookstore](https://github.com/Uniandes-isis2603/bookstore-back/wiki/Historias-de-usuario)

# ![](https://i.imgur.com/TxMjTBf.png) <a id="1Definicion"></a> Definición
Las historias de usuario (HU) son un formato conveniente para expresar las características deseadas de un sistema que le aportan valor a un usuario. Las historias de usuario se crean de tal forma que sean comprensibles para todos los stakeholders (clientes, usuarios, desarrolladores).

Las HU son simples y se pueden escribir en varios niveles de granularidad para refinarlas progresivamente.

Una HU se debe poder escribir en una tarjeta de 7 x 12 cm, usando el siguiente patrón lingüístico:

"**Como** un _rol del usuario_ **quiero** _objetivo_ **para** _beneficio_".

Un ejemplo de una historia de usuario sería el siguiente:

|**Ver comentarios de restaurantes cercanos**|
|---|
|Como _cliente del restaurante_ quiero ver los comentarios de restaurantes cercanos para reservar uno en particular.|

Como se observa, las tarjetas no están destinadas a capturar toda la información del requisito. De hecho, se usan deliberadamente tarjetas pequeñas con espacio limitado para promocionar la brevedad. Una tarjeta debe contener algunas frases que capturen la esencia o la intención de un requisito. Por tanto, sirven como marcador de posición para discusiones más detalladas que tomarán lugar entre las partes interesadas, el propietario del producto y el equipo de desarrollo.

Una historia de usuario también contiene información de confirmación en forma de criterios de aceptación (CA), los cuales aclaran el comportamiento deseado. Estos son utilizados por el equipo de desarrollo para comprender mejor qué construir y probar y
por el propietario del producto para confirmar que la historia de usuario se ha implementado a satisfacción.

Si el anverso de la carta tiene una descripción de unas pocas líneas de la HU, el reverso de la carta podría especificar criterios de aceptación, que pueden expresarse como pruebas de aceptación de alto nivel.

Los CA son una forma importante de capturar y comunicar, desde la perspectiva del propietario del producto, cómo determinar si la historia se ha implementado correctamente.

Los CA también pueden ser una forma útil de crear historias iniciales y refinarlas a medida que se conocen más detalles. Este enfoque a veces se denomina Acceptance-Test-Driven Development (ATTD)(ATTD).

A continuación se presenta una historia de usuario junto con sus criterios de aceptación.

|**Subir archivos**|
|---|
|Como usuario de la Wiki quiero subir archivos para compartirlos con mis colegas.|

|**Criterios de aceptación**|
|---|
|- Verificar que los documentos de texto tengan extensiones .txt o .doc|
|- Verificar que las imágenes tengan extensiones .jpg, .gif o .png|
|- Verficar que los videos tengan extensión .mp4 y un tamaño menor a 1 GB|
|- Verificar que el archivo no tenga restricciones de DRM|
 
# ![](https://i.imgur.com/TxMjTBf.png) <a id="2Criterios"></a> Criterios `INVEST `para revisar las HU



¿Cómo sabemos si las historias que hemos escrito son buenas historias? 

[Bill Wake](https://www.coursera.org/lecture/uva-darden-getting-started-agile/bill-wake-on-invest-sWjWB) ha definido seis criterios útiles
para evaluar si nuestras historias son aptas para el uso previsto o si requieren algun trabajo adicional.

Esos criterios están representados por el acrónimo INVEST: Independiente, Negociable, Valiosa, Estimable, Pequeña (_small_: del tamaño adecuado) y comprobable (_testeable_). 

## <a id="21independiente"></a> Independiente

Las historias de usuario deben ser independientes o muy poco acopladas entre sí. Las historias que muestran un alto grado de interdependencia complican la estimación, la priorización y la planificación.

## <a id="22Negociable"></a> Negociable

Los detalles de las historias deberían ser negociables. Las historias no son un contrato escrito; por el contrario, las historias son marcadores de posición para las conversaciones donde se negociarán los detalles. Las buenas historias capturan claramente la esencia de la funcionalidad deseada pero dejan espacio para que el propietario del producto, las partes interesadas y el equipo negocien los detalles.

##  <a id="23Valiosa"></a> Valiosa

Las historias deben ser valiosas para un cliente, un usuario o ambos. Si una historia no es valiosa para ninguno de los dos, no pertenece al listado de productos a desarrollar (_product backlog_). 

## <a id="24Estimable"></a>Estimable

Las historias deben ser estimables por el equipo que las diseñará, construirá y probará. Las estimaciones proporcionan una indicación del tamaño y, por lo tanto, del esfuerzo y costo de las historias. De este modo, las historias más grandes requieren más esfuerzo y, por lo tanto, cuesta más dinero desarrollarlas que las historias más pequeñas.

##  <a id="25Tamanio">Pequeña (del tamaño adecuado)

Las historias deben tener el tamaño adecuado para cuando planeamos trabajar en ellas. Si estamos haciendo un sprint de varias semanas, queremos trabajar en varias historias que tengan unos pocos días de duración. Si tenemos un sprint de dos semanas, no queremos una historia de dos semanas, porque el riesgo de no terminar la historia es alto.

## <a id="26Probar">Que se pueda probar

Las historias deben ser comprobables de forma binaria: pasan o no sus correspondientes pruebas. Ser `testable `significa tener buenos criterios de aceptación (relacionados con las condiciones de satisfacción) asociados con la historia.

# ![](https://i.imgur.com/TxMjTBf.png) <a id="3Ejemplo"></a>Ejemplo


## Historias de usuario: iteración 1

| Id   | Nombre                                              | Resumen                                                                                                      | Detalla | Revisa |
| ---- | --------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ | ------- | ------ |
| [HU01](#hu01)| Consultar catálogo de álbumes                       | Como usuario visitante quiero navegar el catálogo de los álbumes para escoger los que más me interesan       |  José       |   Cesar     |
| HU02 | Consultar la información detallada de un álbum      | Como usuario visitante quiero ver el detalle de un álbum para ampliar la información sobre él                |    José     |     Cesar   |


### <a id="hu01"></a>Detalle HU01 Consultar catálogo de álbumes


|Elemento|Descripción|
|---|---|
|Consultar catálogo de álbumes| Como usuario visitante quiero navegar el catálogo de los álbumes para escoger los que más me interesan|
|Criterios de aceptación| |
||Al entrar a la página, el usuario debe ver un listado con todos los álbumes registrados. Por cada álbum su carátula.|
||Debe mostrarse un mensaje donde se indique el total de álbumes que se están mostrando en la listado.|	
||En caso de no haber ningún álbum, debe indicarse que hay 0 álbumes para mostrar.|
||Al entrar a la pagina desde un dispositivo con pantalla pequeña, debe mostrarse el contenido de tal forma que se ajuste al tamaño de la misma|
|Mockup| ![](https://github.com/Uniandes-isis2603/recursos-isis2603/blob/master/imagesWiki/albumes.PNG) |

|Criterio| Revisión|
|--|---|
| `Independiente`| La HU es independiente se centra en el listado de álbumes, no se necesita tener implementada otra HU.| 
|`Negociable`| En la HU se puede negociar la forma del despliegue. Qué tanto detalle del álbum, fotos, video? Paginación?  |
| `Valiosa`| La HU es la más importante de la aplicación dado que el objetivo es vender los álbumes de los artistas y grupos. |
| `Estimable`:|  Su especificidad hace que sea perfecta para ser desarrollada en el marco de una semana|
| `Pequeña`| La HU es esta perfectamente acotada y pueden evaluarse su alcance y duración.|
| "`Testeable`"| La HU se puede probar fácilmente a partir de datos de álbumes y comprobar el despliegue.|

### `Notas`:
* En el mockup no se muestra el total de álbumes como se describe en el detalle.
* No se muestra cómo se va a navegar entre los álbumes en caso de que no quepan en la pantalla.
* No se muestra cómo se vería en una pantalla de teléfono.
		</td>
	</tr>
</tbody>
</table>