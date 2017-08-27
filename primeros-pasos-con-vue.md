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



