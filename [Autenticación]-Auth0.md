[Auth0](https://auth0.com) es una plataforma de autenticación y autorización que puede ser usada en páginas web, aplicaciones móviles e incluso dispositivos IoT. Una de sus principales ventajas es su integración con las APIs de Google, Facebook, Github, entre otros, con el fin de que un usuario no deba crear una cuenta desde cero pudiendo usar el mismo nombre de usuario y contraseña ya usados en estas plataformas de terceros.

El presente tutorial muestra los aspectos básicos de la utilización de Auth0 con React con el fin de asegurar las diferentes secciones que conforman una página web. Ingresando a la [página oficial](https://auth0.com) también podrá encontrar tutoriales para usar Auth0 desde el backend, con el fin proteger el acceso a la información contenida en el servidor, o desde Meteor, para que pueda ser integrada en todos los aspectos de una aplicación multiplataforma.

Para empezar a usar Auth0 primero debe crear una cuenta en su [página oficial](https://auth0.com). 

Luego, en el menú de la parte izquierda seleccione la opción Applications. Cree una nueva aplicación de tipo Single Page Application. 

Guarde los valores de los campos `Domain` y `Client ID`. Estos los necesitará más adelante.

En las configuraciones (Settings) de la aplicación ingrese http://localhost:3000 en las opciones `Allowed Callback URLs`, `Allowed Logout URLs` y `Allowed Web Origins`.

### Crear aplicación React

Con el comando `npx create-react-app auth0` cree una nueva aplicación. Ingrese a la nueva carpeta creada y ejecute la aplicación con `npm start`.

### Instalar dependencias

Instale las dependencias `react-router-dom` y `auth0-spa-js` con el comando `npm install react-router-dom @auth0/auth0-spa-js`.

### React Router History

Se debe permitir a React que redireccione el navegador de forma automática teniendo en cuenta el historial de navegación. Para ello dentro de la carpeta `src` cree una carpeta denominada `utils`, y allí cree un nuevo archivo `history.js` con el siguiente contenido:


```javascript
// src/utils/history.js

import { createBrowserHistory } from "history";
export default createBrowserHistory();
```

### React wrapper 

Las funciones de logueo, desconexión e identificación del usuario se harán en el archivo `src/react-auth0-spa.js` que tiene el siguiente código:

```javascript
// src/react-auth0-spa.js
import React, { useState, useEffect, useContext } from "react";
import createAuth0Client from "@auth0/auth0-spa-js";

const DEFAULT_REDIRECT_CALLBACK = () =>
  window.history.replaceState({}, document.title, window.location.pathname);

export const Auth0Context = React.createContext();
export const useAuth0 = () => useContext(Auth0Context);
export const Auth0Provider = ({
  children,
  onRedirectCallback = DEFAULT_REDIRECT_CALLBACK,
  ...initOptions
}) => {
  const [isAuthenticated, setIsAuthenticated] = useState();
  const [user, setUser] = useState();
  const [auth0Client, setAuth0] = useState();
  const [loading, setLoading] = useState(true);
  const [popupOpen, setPopupOpen] = useState(false);

  useEffect(() => {
    const initAuth0 = async () => {
      const auth0FromHook = await createAuth0Client(initOptions);
      setAuth0(auth0FromHook);

      if (window.location.search.includes("code=") &&
          window.location.search.includes("state=")) {
        const { appState } = await auth0FromHook.handleRedirectCallback();
        onRedirectCallback(appState);
      }

      const isAuthenticated = await auth0FromHook.isAuthenticated();

      setIsAuthenticated(isAuthenticated);

      if (isAuthenticated) {
        const user = await auth0FromHook.getUser();
        setUser(user);
      }

      setLoading(false);
    };
    initAuth0();
    // eslint-disable-next-line
  }, []);

  const loginWithPopup = async (params = {}) => {
    setPopupOpen(true);
    try {
      await auth0Client.loginWithPopup(params);
    } catch (error) {
      console.error(error);
    } finally {
      setPopupOpen(false);
    }
    const user = await auth0Client.getUser();
    setUser(user);
    setIsAuthenticated(true);
  };

  const handleRedirectCallback = async () => {
    setLoading(true);
    await auth0Client.handleRedirectCallback();
    const user = await auth0Client.getUser();
    setLoading(false);
    setIsAuthenticated(true);
    setUser(user);
  };
  return (
    <Auth0Context.Provider
      value={{
        isAuthenticated,
        user,
        loading,
        popupOpen,
        loginWithPopup,
        handleRedirectCallback,
        getIdTokenClaims: (...p) => auth0Client.getIdTokenClaims(...p),
        loginWithRedirect: (...p) => auth0Client.loginWithRedirect(...p),
        getTokenSilently: (...p) => auth0Client.getTokenSilently(...p),
        getTokenWithPopup: (...p) => auth0Client.getTokenWithPopup(...p),
        logout: (...p) => auth0Client.logout(...p)
      }}
    >
      {children}
    </Auth0Context.Provider>
  );
};
```



## Componente NavBar

Un componente NavBar será el encargado de mostrar los botones "Login" y "Logout". Para crear este comoponente, dentro de la carpeta `src` cree una nueva carpeta `components` y luego un archivo `NavBar.js` con el siguiente contenido:

```javascript
// src/components/NavBar.js

import React from "react";
import { useAuth0 } from "../react-auth0-spa";

const NavBar = () => {
  const { isAuthenticated, loginWithRedirect, logout } = useAuth0();

  return (
    <div>
      {!isAuthenticated && (
        <button onClick={() => loginWithRedirect({})}>Log in</button>
      )}

      {isAuthenticated && <button onClick={() => logout()}>Log out</button>}
    </div>
  );
};

export default NavBar;
```

### Envolver el compoente principal

El componente principal de la aplicación se debe envolver con el compoentente `Auth0Provider`. Para esto, mofique el archivo `src/index.js` así:

```javascript
// src/index.js

import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import * as serviceWorker from "./serviceWorker";
import { Auth0Provider } from "./react-auth0-spa";
import config from "./auth_config.json";
import history from "./utils/history";

// A function that routes the user to the right place
// after login
const onRedirectCallback = appState => {
  history.push(
    appState && appState.targetUrl
      ? appState.targetUrl
      : window.location.pathname
  );
};

ReactDOM.render(
  <Auth0Provider
    domain={config.domain}
    client_id={config.clientId}
    redirect_uri={window.location.origin}
    onRedirectCallback={onRedirectCallback}
  >
    <App />
  </Auth0Provider>, 
  document.getElementById("root")
);

serviceWorker.unregister();
```

### Datos de configuración

Los datos de configuración de Auth0 se deben establecer en un archivo. Para ello, cree un archivo `src/auth_config.json` con el siguiente contenido:

```json
{
  "domain": "YOUR_DOMAIN",
  "clientId": "YOUR_CLIENT_ID"
}
```

No olvide cambiar los valores de los atributos `domain` y `clientId` por los que guardó cuando creó la applicación en la página de Auth0.

## Componente principal

Ahora, el componente `NavBar`

 que se creó previamente, será parte del componente principal. Modifique el archivo `src/App.js` así:

```javascript
// src/App.js

import React from "react";
import NavBar from "./components/NavBar";
import { useAuth0 } from "./react-auth0-spa";

function App() {
  const { loading } = useAuth0();

  if (loading) {
    return <div>Loading...</div>;
  }

  return (
    <div className="App">
      <header>
        <NavBar />
      </header>
    </div>
  );
}

export default App;
```

Ejecute la aplicación y analice su comportamiento.

## Reto


