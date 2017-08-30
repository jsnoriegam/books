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

Es recomendable crear un servicio para las operaciones de de autenticación.

> El token es almacenado utilizando localStorage \([https://developer.mozilla.org/es/docs/Web/API/Window/localStorage](https://developer.mozilla.org/es/docs/Web/API/Window/localStorage)\)

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
        // Es necesario configurar el router para verificar la validez del token en cada cambio de página
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

        // Interceptamos todas las respuestas y borramos el token en caso de un error 401
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

Es necesario indicar al router que rutas necesitan verificar autenticación, para lo cual usamos **meta**:

```js
import login from './pages/login.vue'
import dashboard from './pages/dashboard.vue'
import peliculas from './pages/peliculas.vue'
import personas from './pages/personas.vue'

const routes = [
    {
        name: 'login',
        path: '/login',
        component: login
    },
    {
        name: 'dashboard',
        path: '/',
        component: dashboard,
        meta: { auth: true }
    },
    {
        name: 'peliculas',
        path: '/peliculas',
        component: peliculas,
        meta: { auth: true }
    },
    {
        name: 'personas',
        path: '/personas',
        component: personas,
        meta: { auth: true }
    }
];

export default routes
```

Debemos hacer las modificaciones necesarias a main.js

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

import app from './app.vue'

import routes from './routes'

import authService from './services/auth-service'

Vue.use(VueRouter);

const router = new VueRouter({
    routes
});

authService.configRouter(router);
authService.configAxios();

new Vue({
    router,
    render: (h) => h(app)
}).$mount('#app');
```

Por último elaboremos una página de login:

```html
<template>
    <div>
        <h3>Ingresar al sistema</h3>
        <div class="row">
            <div class="col-md-4">
                <form @submit.prevent="login">
                    <div class="form-group">
                        <label for="usuario_username">Nombre de usuario:</label>
                        <input type="text" class="form-control" id="usuario_username" v-model="usuario.username">
                    </div>
                    <div class="form-group">
                        <label for="usuario_password">Password:</label>
                        <input type="password" class="form-control" id="usuario_password" v-model="usuario.password">
                    </div>
                    <div class="form-group">
                        <button type="submit" class="btn btn-primary">Ingresar</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import authService from '../services/auth-service'

export default {
    data() {
        return {
            usuario: {}
        }
    },
    methods: {
        login() {
            authService.autenticar(this.usuario.username, this.usuario.password).then(response => {
                this.$router.push({ name: 'dashboard' });
            });
        }
    }
}
</script>
```

>



