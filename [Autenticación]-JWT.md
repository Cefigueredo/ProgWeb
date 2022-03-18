JWT es un estándar para compartir información entre un cliente y un servidor de forma segura. El mecanismo de trabajo es el siguiente:
- El cliente realiza una solicitud de autenticación al servidor enviando, en la mayoría de los casos, un usuario y una contraseña.
- El servidor valida que los datos de autenticación coincidan con sus registros, genera un token y lo retorna al usuario en forma de cadena de texto.
- El cliente debe asegurarse de almacenar ese token para futuras solicitudes a través de mecanismos como _local storage_ o _session storage_.
- En futuras solicitudes, el servidor verificará que el token proporcionado por el cliente coincida con el generado previamente durante la etapa de login.

Para realizar este tutorial debe crear una nueva aplicación con Node.js y Express: `express --view=pug jwt`. 

Ingrese a la nueva carpeta generada e instale las dependencias. 

Para integrar JWT en su aplicación Node.js debe instalar el paquete `jsonwebtoken` mediante `npm install jsonwebtoken`. 

En un ambiente real, debe crear una variable de entorno la cual debe contener una _secret key_ única que será usada para la generación y validación de tokens. En este tutorial, por facilidad almacenaremos la _secret key_ en un archivo dentro de la aplicación llamado `config.js`:

```javascript
module.exports = {
  secret: 'uniandesisiswebjwt'
};
```

Se debe crear otro archivo llamado `middleware.js` el cual tendrá como función principal verificar que el token enviado por el usuario si coincida con el generado durante la fase de login:

```javascript
let jwt = require( 'jsonwebtoken' );
const config = require( './config.js' );

// Función encargada de realizar la validación del token y que es directamente consumida por server.js
let checkToken = ( req, res, next ) => {
  
  // Extrae el token de la solicitud enviado a través de cualquiera de los dos headers especificados
  // Los headers son automáticamente convertidos a lowercase
  let token = req.headers[ 'x-access-token' ] || req.headers[ 'authorization' ];
  
  // Si existe algún valor para el token, se analiza
  // de lo contrario, un mensaje de error es retornado
  if( token ) {

    // Si el token incluye el prefijo 'Bearer ', este debe ser removido
    if ( token.startsWith( 'Bearer ' ) ) {
        token = token.slice(7, token.length );
        // Llama la función verify del paquete jsonwebtoken que se encarga de realizar la validación del token con el secret proporcionado
        jwt.verify( token, config.secret, ( err, decoded ) => {
      
        // Si no pasa la validación, un mensaje de error es retornado
        // de lo contrario, permite a la solicitud continuar
        if( err ) {
          return res.json( {
            success: false,
            message: 'Token is not valid'
          } );
        } else {
          req.decoded = decoded;
          next();
        }
      } );
    }
  } else {
    
    return res.json( {
      success: false,
      message: 'Auth token is not supplied'
    } );

  }

};

module.exports = {
  checkToken: checkToken
}
```

La parte más importante del script anterior es la línea `jwt.verify( token, config.secret, ( err, decoded ) => { ... }`. En esta `jsonwebtoken` recibe como parámetro el token enviado por el usuario y la _secret key_ especificada, en este caso en el script `config.js`. Si la verificación del token es exitosa, la función callback deberá incluir el llamado a la función `next()` indicándole a la aplicación que la solicitud para ese cliente debe continuar.

Finalmente, se crea la aplicación principal dentro del archivo `handlegenerator.js`:

```javascript

let jwt = require( 'jsonwebtoken' );
let config = require( './config' );

// Clase encargada de la creación del token
class HandlerGenerator {

  login( req, res ) {
    
    // Extrae el usuario y la contraseña especificados en el cuerpo de la solicitud
    let username = req.body.username;
    let password = req.body.password;
    
    // Este usuario y contraseña, en un ambiente real, deben ser traidos de la BD
    let mockedUsername = 'admin';
    let mockedPassword = 'password';

    // Si se especifico un usuario y contraseña, proceda con la validación
    // de lo contrario, un mensaje de error es retornado
    if( username && password ) {

      // Si los usuarios y las contraseñas coinciden, proceda con la generación del token
      // de lo contrario, un mensaje de error es retornado
      if( username === mockedUsername && password === mockedPassword ) {
        
        // Se genera un nuevo token para el nombre de usuario el cuál expira en 24 horas
        let token = jwt.sign( { username: username },
          config.secret, { expiresIn: '24h' } );
        
        // Retorna el token el cuál debe ser usado durante las siguientes solicitudes
        res.json( {
          success: true,
          message: 'Authentication successful!',
          token: token
        } );

      } else {
        
        // El error 403 corresponde a Forbidden (Prohibido) de acuerdo al estándar HTTP
        res.send( 403 ).json( {
          success: false,
          message: 'Incorrect username or password'
        } );

      }

    } else {

      // El error 400 corresponde a Bad Request de acuerdo al estándar HTTP
      res.send( 400 ).json( {
        success: false,
        message: 'Authentication failed! Please check the request'
      } );

    }

  }

  index( req, res ) {
    
    // Retorna una respuesta exitosa con previa validación del token
    res.json( {
      success: true,
      message: 'Index page'
    } );

  }
}

module.exports = HandlerGenerator;
``` 

Finalmente, modifique el archivo `routes/index.js` con el siguiente código:

```javascript
var express = require('express');
var router = express.Router();

var HandlerGenerator = require("../handlegenerator.js");
var middleware = require("../middleware.js");

HandlerGenerator = new HandlerGenerator();

/* GET home page. */
router.get('/', middleware.checkToken, HandlerGenerator.index);

router.post( '/login', HandlerGenerator.login);

module.exports = router;

```
Tenga en cuenta que:

- `POST /login`: Debe ser usado para enviar los datos de autenticación (usuario y contraseña) en formato JSON el cuerpo de solicitud. Dentro de la respuesta, si los datos proporcionados por el cliente son correctos, se retornará el token que deberá ser almacenado por el cliente en local o session storage. 
- `GET /`: Endpoint genérico el cuál incluye una cadena de handlers a ser ejecutados en cascada durante su llamado. El primero corresponde a la verificación del token, si es adecuado la aplicación procederá a llamar la siguiente función de la cadena.

Este tutorial no incluye la conexión con bases de datos para realizar la verificación del nombre de usuario y contraseña, por lo que al iniciar la aplicación solo estará habilitado para generar tokens usando `admin` y `password` como credenciales.

Adicionalmente, durante el llamado al método jwt.sign(), en el archivo handlegenerator.js, se están especificando 3 parámetros. El primero corresponde al _payload_ en formato JSON a ser usado para generar el token. En este caso solo se está enviando el nombre de usuario, pero usted adicionalmente puede incluir la contraseña así como información adicional para hacer más robusta la generación. El segundo parámetro corresponde a la secret key de la que ya hemos hablado previamente y que debe ser especificada en la variable de entorno o el archivo `config.js`. El tercer parámetro corresponde a configuraciones adicionales para la generación del token, en este caso solo estamos especificando el tiempo de expiración. Para mayor detalle con respecto a las opciones de configuración disponibles puede consultar la [documentación oficial](https://github.com/auth0/node-jsonwebtoken).


### Reto
Modifique la aplicación desarrollada en los tutoriales anteriores para que involucre autenticación usando JWT. Para esto debe:
- Modificar el servidor / backend para conectarse con una base de datos (tipo SQL o MongoDB) la cual contiene los usuarios y contraseñas de la aplicación. Implemente un mecanismo adicional para no tener que almacenar las contraseñas como texto plano ya que es considerada una mala práctica en el diseño de sistemas de autenticación.
- Cree un total de 2 roles en la aplicación dentro de un esquema de autorización adecuado para el acceso de diferentes niveles de información. El primer rol tendrá acceso de lectura a todos los enpoints mientras que el segundo rol tendrá acceso de escritura a todos los enpoints.