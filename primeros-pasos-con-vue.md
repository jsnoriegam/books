# Primeros pasos con Vue.js

## 3.1 main.js

El primer paso para iniciar nuestra aplicación es crear el index.html, la página que iniciará todo:

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <title></title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link href="css/style.css" rel="stylesheet">
    </head>
    <body>
        <div id="app"></div>
        <script src="dist/app.js"></script>
    </body>
</html>
```

Puesto que vamos a utilizar la estructura base propuesta por el gulpfile.js, como segundo paso debemos crear un directorio llamado src y dentro los archivos: main.js, routes.js y app.vue.

main.js será el archvo de arranque de nuestra aplicación:

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

import app from './app.vue'

import routes from './routes'

Vue.use(VueRouter);

const router = new VueRouter({
    routes
});

new Vue({
    router,
    render: (h) => h(app)
}).$mount('#app');//id del elemento root
```

routes.js contendrá las rutas de nuestra aplicación:

```js
import dashboard from './pages/dashboard.vue'
import pagina1 from './pages/pagina1.vue'
import pagina2 from './pages/pagina2.vue'

const routes = [
    {
        name: 'dashboard',
        path: '/',
        component: dashboard
    },
    {
        name: 'pagina1',
        path: '/pagina1',
        component: pagina1
    },
    {
        name: 'pagina2',
        path: '/pagina2',
        component: pagina2
    }
];

export default routes
```

Y por último nuestra página inicial app.vue:

```html
<template>
    <div>
        <div id="menu">
            <router-link :to="{ name: 'dashboard' }">Dashboard</router-link>
            <router-link :to="{ name: 'pagina1' }">Pagina 1</router-link>
            <router-link :to="{ name: 'pagina2' }">Pagina 2</router-link>
        </div>
        <div id="content">
            <router-view></router-view>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {

        }
    }
}
</script>
```

Como podemos ver vamos a trabajar con componentes de un solo archivo \([https://vuejs.org/v2/guide/single-file-components.html](https://vuejs.org/v2/guide/single-file-components.html)\)

