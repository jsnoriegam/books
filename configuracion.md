# Configuración

## 2.1 Gulp

Gulp \([https://gulpjs.com](https://gulpjs.com)\) es un sistema de construcción basado en flujos, cuya principal utilidad es la automatización de las tareas.

Para utilizar Gulp además de los paquetes instalados anteriormente en el proyecto debemos instalar el paquete del cliente de linea de comandos de gulp, este paquete debe ser instaldo de manera global, esto lo hacemos con el siguiente comando:

```
npm install gulp-cli -g
```

Esta instalación solo debe ser realizada una vez y servirá para cualquier proyecto futuro.

Si no queremos o no podemos instalar el gulp-cli, tambien es posible utilizar gulp a través de npm configurando el comando en la sección **scripts** del package.json y utilizando el comando:

```
npm run [nombre-del-script]
```

### 2.1.1 Gulpfile.js

Gulp requiere que las tareas sean definidas en un archivo llamado gulpfile.js

Para definir una tarea utilizamos gulp.task\(\[nombre-de-la-tarea\], \[funcion\]\);

Este es un ejemplo de un gulpfile que podemos usar como base para cualquier proyecto, con las tareas básicas necesarias para iniciar el desarrollo:

```js
"use strict"

const gulp = require('gulp');
//Plumber simplifica el manejo de errores en gulp
const plumber = require('gulp-plumber');
//Uglify minifica el javascript generado
const uglify = require('gulp-uglify');
//Crea sourcemaps -necesarios para hacer debug-
//https://www.html5rocks.com/en/tutorials/developertools/sourcemaps/
const sourcemaps = require('gulp-sourcemaps');

const browserify = require('browserify');
const browserifyShim = require('browserify-shim');
//Permite el uso de componentes .vue
const vueify = require('vueify');
//Babel es un transpilador de Ecmascript 2015/16
//Básicamente nos permite utilizar la nueva sintaxis de javascript
//y la transforma a la sintaxis soportada por los navegadores actuales
const babelify = require('babelify');
//Envify me permite utilizar process.env.NODE_ENV dentro de la app
//para determinar si el código se esta ejecutando en producción
const envify = require('envify');
const watchify = require('watchify');
//Source transforma un flujo de texto a un flujo vinyl
//Basicamente hace posible convertir los flujos generados por browserify a flujos soportados por gulp
const source = require('vinyl-source-stream');
const buffer = require('vinyl-buffer');

//BrowserSync se encarga de servir la aplicación
const bs = require('browser-sync').create();

function createBundler(args) {
    return browserify('./src/main.js', args)
        .transform(browserifyShim)
        .transform(vueify)
        .transform(babelify)
        .transform(envify)
}

function bundle(bundler, dest = 'dist') {
    return bundler.bundle()
        .pipe(plumber())
        .pipe(source('app.js'))
        .pipe(buffer())
        .pipe(sourcemaps.init())
        .pipe(uglify())
        .pipe(sourcemaps.write('.'))
        .pipe(gulp.dest(dest))
}

const browserifyconf = {};

gulp.task('build', function() {
    process.env.NODE_ENV = 'production';
    let args = Object.assign({}, browserifyconf, { debug: true });
    const bundler = createBundler(args);
    return bundle(bundler);
});

gulp.task('watch', function() {
    process.env.NODE_ENV = 'development';
    let args = Object.assign({}, watchify.args, browserifyconf, { debug: true });
    const bundler = createBundler(args).plugin(watchify);
    bundle(bundler, 'dev');
    bundler.on('update', function() {
        bundle(bundler, 'dev').pipe(bs.reload({ stream: true }));
    });
});

gulp.task('serve', ['watch'], function() {
    bs.init({
        server: {
            baseDir: './',
            index: 'index.dev.html'
        },
        serveStatic: [{
            route: '/dist',
            dir: './dev'
        }]
    });
});
```

Este gulpfile establece que las fuentes del proyecto estarán en el directorio **src **y que el empaquetado de desarrollo estará en el directorio **dev** mientras que el empaquetado final estará en el directorio **dist**.  Además nos indica que utilizará el archivo **main.js** como script de inicio.

### 2.1.3 Browserify y Babel

Browserify es una herramienta que básicamente nos permite hacer **require** a los paquetes de node \(npm\) desde el cliente del navegador Ej.:

```js
var Vue = require('vue');
```

Y además nos permite crear bundles que mejorarán el rendimiento de nuestra aplicación web.

Browserify utiliza package.json para referenciar los paquetes,:

```json
"browser": {
    "vue": "vue/dist/vue.common.js",
    "jquery": "jquery/dist/jquery.slim.js"
},
"browserify-shim": {}
```

La sección **browser** se utiliza para cambiar el archivo que será referenciado al momento de utilizar require o import.

> jquery.slim es una versión de jquery que no contine los metodos para ajax, animaciones , ni los obsoletos

La sección **browserify-shim** nos permite referenciar paquetes que no son compatibles con browserify para habilitarlos y definir sus dependencias \(solamente si usamos **.transform\(browserifyShim\)**\)

Browserify utiliza Babel \([https://babeljs.io](https://babeljs.io)\) a través del plugin babelify para transformar nuestro código al código que el navegador entiende, pero es necesario decirle como, y para esto debemos crear un archivo llamado .babelrc con el siguiente contenido:

```json
{
    "presets": [
        "env"
    ]
}
```

Para mas opciones de configuración ir a: [https://babeljs.io/docs/plugins/preset-env](https://babeljs.io/docs/plugins/preset-env)

