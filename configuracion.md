# Configuración

## 2.1 Gulpfile

Gulp requiere que las tareas sean definidas en un archivo llamado gulpfile.js

Para definir una tarea utilizamos gulp.task\(\[nombre-de-la-tarea\], \[funcion\]\);

Este es un ejemplo de un gulpfile que podemos usar como base para cualquier proyecto, con las tareas básicas necesarias para iniciar el desarrollo:

```js
"use strict"

const gulp = require('gulp');
const plumber = require('gulp-plumber');
//Uglify minifica el javascript generado
const uglify = require('gulp-uglify');
const sourcemaps = require('gulp-sourcemaps');

const browserify = require('browserify');
const browserifyShim = require('browserify-shim');
const vueify = require('vueify');
//Babel es un transpilador de Ecmascript 2015/16
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
        bundle(bundler);
        bs.reload();
    });
});

gulp.task('serve', ['watch'], function() {
    bs.init({
        server: {
            baseDir: './',
            index: 'index.dev.html'
        }
    });
});
```

### 2.1.1 .babelrc

Browserify utiliza Babel a través del plugin babelify para transformar 

```json
{
    "presets": [
        "env"
    ]
}
```



