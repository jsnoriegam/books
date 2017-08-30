# Trabajando con JWT

## 6.1 jwt-decode

Para trabajar con JWT es indispensable pbtener la información que esta codificada dentro del token, para esto necesitamos instalar un nuevo paquete:

```
npm install jwt-decode --save
```

Utilizar este paquete es bastante simple:

```js
import jwtDecode from 'jwt-decode'

let token = ... //Obtenemos el token

let tokenDecoded = jwtDecode(token);
```

## 6.2 AuthService

```js
import axios from 'axios'
import jwtDecode from 'jwt-decode'

let api_url = 'http://localhost:5000/api/v1';

function guardarToken(token) {
    localStorage.setItem('__token__', token);
}

function recuperarToken(token) {
    return localStorage.getItem('__token__');
}

function borrarToken() {
    localStorage.removeItem('__token__');
}

class AuthService {
    autenticar(username, password) {
        return axios.post(`${ api_url }/auth/token`, {
            username: username,
            password: password
        }).then(response => {
            guardarToken(response.data.token);
        });
    }

    cerrarSesion() {
        borrarToken();
    }

    obtenerToken() {
        return recuperarToken();
    }

    obtenerTokenDecodificado() {
        let token = recuperarToken();
        return jwtDecode(token);
    }

    tokenValido() {
        let token = recuperarToken();
        if(!token) {
            return false;
        }
        let tokenDecodificado = jwtDecode(token);
        let ahora = Date.now() / 1000;
        return tokenDecodificado.exp > ahora;
    }

    configRouter(router) {
        router.beforeEach((to, from, next) => {
            if (to.matched.some(record => record.meta.auth)) {
                if(!this.tokenValido()) {
                    next({ name: 'login' });
                } else {
                    next();
                }
            } else {
                next();
            }
        });
    }

    configAxios() {
        // Interceptamos todas las peticiones (excepto /auth/token) y le agregamos el encabezado Authorizarion
        axios.interceptors.request.use((request) => {
            if(`${ api_url }/auth/token` !== request.url && 'OPTIONS' !== request.method.toUpperCase()) {
                let token = this.obtenerToken();
                if(token) {
                    if(!request.headers['Authorization']) {
                        request.headers['Authorization'] = 'Bearer ' + token;
                    }
                }
            }
            return request;
        }, (error) => Promise.reject(error));
        
        // Interceptamos todas las respuestas
        axios.interceptors.response.use((response) => {
            if(response.status === 401) {
                borrarToken();
                return Promise.reject(response);
            } else {
                return Promise.resolve(response);
            }
        }, (error) => Promise.reject(error));
    }
}

export default new AuthService();
```





