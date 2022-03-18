En Javascript, existen los llamados asíncronos. Esto significa que es posible que una parte del código se llame antes de se reciban los resultados de un llamado anteriormente hecho. Esto se conoce como programación asíncrona que permite que una unidad corra aparte del thread principal (esto no significa que Javascript sea multithread, ya que no lo es. Simplemente significa que se pueden hacer llamados largos sin bloquear el thread principal) . Cuando el trabajo se termina, se le notifica al thread principal de que se terminó el trabajo.

![Programación sincrónica, por threads y asincrónica](https://i.imgur.com/3JV5a6m.png)

En el código asíncrono, vemos como otras tareas se pueden ir ejecutando mientras se lleva a cabo el proceso de otra tarea. Esto implica que en un momento dado podemos retornar el valor de una función antes de que se nos consigan los datos que necesitamos. 

Ejemplo: 

Creamos un archivo .js y corremos el siguiente código

```
const start = () => {
  console.log("Iniciando");
  let x = 0;
  x = 1;
  setTimeout(() => {
    x += 100;
    console.log("Se obtuvo información", x);
  }, 1000);

  console.log("Terminado", x);
};

start();

```

En este ejemplo simulamos la obtención de información de un servicio que se obtiene después de un segundo (setTimeout). Cuando se recolecta la información se muestra en consola y se cambia el número de la variable **x**. 
En programación sincrónica (si todas las tareas fueran síncronas y se ejecutaran secuencialmente), se verían los console.log() de la siguiente manera

1.	Iniciando
2.	Se obtuvo información 101
3.	Terminado 101

Sin embargo, en Javascript, los console.log() se ven de la siguiente manera:
1.	Iniciando
2.	Terminado 1
3.	Se obtuvo información 101

![Console.log() asincrónico](https://imgur.com/tuYiZaO.png)

La asincronía puede llegar a tener varios beneficios, como el desempeño de la aplicación. Sin embargo, puede ser desastrosa si no se maneja debidamente. En el anterior caso, nunca hubiéramos recibido correctamente el valor de **x**, pues este es retornado (en este caso imprimido) antes de que se obtenga la información del servicio externo.
Para arreglar el anterior problema, existen 3 soluciones distintas. Por simplicidad, los llamaremos opción _**callback**_, opción _**Promise**_ y opción _**async/await**_.
Las tres soluciones hacen exactamente lo mismo. Sin embargo, se han creado distintas notaciones para resolver el error del ejemplo anteriormente mencionado (Siendo callback el más viejo y async/await el más nuevo).

# ¿Qué es asíncrono y qué no? 

Antes de ver las opciones de cómo volver código asíncrono en código síncrono, veamos qué es síncrono en Javascript y qué no

* Las funciones de alto nivel son _**síncronas**_ : Funciones que obtienen funciones como argumento y utilizan las funciones recibidas o retornan una función.
Las funciones que son aceptadas como argumento son funciones _**callback**_.
Funciones de alto nivel que ya deberían conocer: if, for, forEach, while, funciones sobre arrays (.map, .reduce, .sort, etc..). Para más información sobre funciones de alto nivel: http://web.archive.org/web/20120828191718/http://www.spheredev.org/wiki/Higher-order_programming_in_JavaScript

```diff
! Nota: Las funciones de alto nivel son un aspecto de programación funcional:
http://www.slideshare.net/tmont/introduction-to-functional-programming-in-javascript
```



* Los llamados a servicios externos son _**asíncronos**_ (todos los llamados de peticiones HTTP como Ajax, fetch, módulo HTTP de angular, etc…)

* Los llamados a bases de datos (aunque las bases de datos sean locales) son _**asíncronos**_ . MongoDB maneja las promesas por nosotros (retornando promesas por cada llamado), pero aún así no deja de ser asíncrono.

* Los llamados a eventos de usuario son _**asíncronos**_ . Por ejemplo un click, ya que no sabemos cuando ocurrirá. Por esto en Javascript puro añadimos un eventListener y añadimos una función callback
```
document.getElementById('button').addEventListener('click', () => {
  //item clicked
})
```


# Opción Callback

En esta opción pasaremos una función como parámetro a otra función, para luego ser debidamente invocada dentro de la función externa para completar la rutina.
### Ejemplo

![Ejemplo callback](https://imgur.com/l6zfjMH.png)

Se cambiaron varias partes del código, entonces veamos qué significa cada parte cambiada y la nueva función _**callback**_

1. La función externa, en este caso _**start()**_, recibe un nuevo parámetro llamado _**callback**_(este parámetro se puede llamar como se desee, pero por notación y simplicidad es mejor llamarlo así). 
2. La función _**callback**_ que pasamos como parámetro llama de vuelta el valor _**x**_
3. Recibimos el llamado de la función como parámetro (en Javascript es posible que las funciones tengan funciones como parámetros y pueden ser retornadas por otra función). En este caso recibimos un solo parámetro del _**callback**_ del paso 2, que fue _**x**_ (ahora llamado _**res**_) e imprimimos el valor. 

Ahora bien, el _**console.log(“Terminado” , x)**_ sigue sin estar sincrónico, por lo cual este se imprimirá antes de  _**res**_ ¿Significa esto que está mal? **No**, nuestro objetivo es obtener la información del servicio externo y retornar ese valor. Ya que tenemos la información en _**res**_, podemos hacer lo que queramos con la información obtenida.

### Otro ejemplo
Este código no se puede probar, pero es una simple receta donde se demuestra tareas asincrónicas
```
const prepararPasta = (callback) => {
  agregarAgua((agua) => {
    agregarSal(agua, (sal) => {
      agregarPasta(agua, sal, (pasta) => {
        servirPasta(pasta, (plato) => {
          callback(plato);
        });
      });
    });
  });
};

prepararPasta(pastaHecha => {
    console.log(pastaHecha);
});

```

¿Qué está pasando? El problema es un _**callback hell**_ .Es cuando tenemos muchos llamados asincrónos y tenemos que llamar funciones dentro de funciones hasta lograr nuestro dato requerido. Para evitar estas confusiones, se creó la opción 2, **promesas**. Sin embargo, era necesario demostrar cómo funcionan los **callbacks**, ya que es la base de volver código asíncrono a código síncrono.

# Opción promesas

Para evitar el _**callback hell**_, se utilizan **promesas**. Esta opción es distinta en donde se **crea una promesa y se** **retorna una promesa** cuando todo el código termine, sin embargo, tiene la misma lógica del callback por dentro. La creación de una promesa recibe dos parámetros, _**resolve y reject**_, aunque estos dos nombres pueden variar lo importante es su significado. El primer parámetro sirve como _**callback**_ cuando la promesa fue cumplida satisfactoriamente y el segundo cuando ocurrió un error. Es decir, o recibimos un value o recibimos un error.
Para crear una promesa solo debemos tener la siguiente sintaxis

```
 new Promise((resolve, reject) => {});

```

Lo que quede dentro del código de la promesa será devuelto una vez se haga el callback con resolve o reject. 

### Ejemplo set timeout con promesas


![Callback con promesas](https://imgur.com/wbGtWxK.png)


1.	Creamos la promesa con sus dos parámetros y le asignamos una variable que será retornada después
2.	Realizamos un callback de resolve() con el valor que queremos o un reject si hubo un error en alguna parte de la operación
3.	Retornamos la _**definición**_ de la promesa. En este caso **no devolvemos el valor de la promesa sino la promesa como tal**
4.	Por último, obtenemos el valor de la promesa. Para esto utilizamos el _**.then()**_, utilizado cuando se completa una promesa y el valor que nos retornó la promesa como una función

![Console.log promesas](https://imgur.com/4uhOFdo.png)

Ahora ya que vimos promesas es más fácil imaginarse como evitar el _**callback hell**_ . Creamos multiples promesas y retornamos la última promesa que necesita todos los datos. Para ejemplificar el ejemplo de hacer pasta tendríamos lo siguiente:

### Ejemplo pasta con promesas 

```
const servirPlato = () => {
  const agregarAgua = new Promise((resolve, reject) => {
    resolve(agua);
  });

  const agregarSal = new Promise((resolve, reject) => {
    agregarAgua.then((agua) => {
      resolve(agua, sal);
    });
  });

  const agregarPasta = new Promise((resolve, reject) => {
    agregarSal.then((agua, sal) => {
      resolve(pasta);
    });
  });

  const terminar = new Promise((resolve, reject) => {
    agregarPasta.then((pasta) => {
      resolve(plato);
    });
  });

  return terminar
};

servirPlato().then((plato) => {
  console.log("plato hecho", plato);
});

```

Acá vemos que todas las promesas están en la misma función de _**servirPlato()**_, por lo cual no es necesario anidar el código y resulta más legible.

# Opción async/await

Es la opción más fácil y la más actualizada. Es una manera de encapsular promesas de una manera más rápida que las anteriores y concisa en cuanto a código. Hay ciertas reglas para tener en cuenta al utilizar async/await. Async/await no pueden vivir el uno sin el otro, por lo cual, si se utiliza async, se tiene que utilizar await. Al hacer un await estamos esperando una _**promesa**_.

### Ejemplo setTimeout con async/await

![async/await](https://imgur.com/6FsY6Js.png)

1.	La función debe tener un async, que devuelve un asyncFunction, representando una función asíncrona que ejecuta el código dentro de la función
2.	Usamos el operador await para esperar una promesa que solo puede ser llamada dentro de una función async. Cuando async termina devuelve a la línea de ejecución de la función await
3.	Creamos una promesa y hacemos el callback de la definición de la promesa
4.	Obtenemos la respuesta de la promesa como una función y mostramos el valor


![Console.log de async/await](https://imgur.com/lSLesps.png)

Con el ejercicio de la pasta

![Ejercicio pasta con async/await](https://imgur.com/zFnPOs0.png)




