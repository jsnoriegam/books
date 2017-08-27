# Instalación

## 1.1 Node.js y NPM

Vue.js no especifica una única forma de instalación, sino que permite al desarrollador escoger la que más se acomode a sus necesidades, pero para un mejor control yo recomiendo utilizar NPM \([https://www.npmjs.com](https://www.npmjs.com)\).

Lo primero que debemos hacer es crear un proyecto de NPM, para lo cual creamos un directorio y en una consola escribimos el siguiente comando:

```
npm init
```

Luego debemos responder las preguntas que npm nos haga:

See \`npm help json\` for definitive documentation on these fields

    npm init
    This utility will walk you through creating a package.json file.
    It only covers the most common items, and tries to guess sensible defaults.

    See `npm help json` for definitive documentation on these fields
    and exactly what they do.

    Use `npm install <pkg> --save` afterwards to install a package and
    save it as a dependency in the package.json file.

    Press ^C at any time to quit.
    name: (Movies.Client) movies-api-client
    version: (1.0.0) 0.0.1
    description: Movies API Client
    entry point: (index.js) main.js
    test command: gulp test
    git repository:
    keywords: Movies Vue.js
    author: Juan S
    license: (ISC) MIT
    About to write to F:\Users\Juansa\Documents\CoreProjects\MoviesSln\Movies.Client\package.json:

    {
      "name": "movies-api-client",
      "version": "0.0.1",
      "description": "Movies API Client",
      "main": "main.js",
      "scripts": {
        "test": "gulp test"
      },
      "keywords": [
        "Movies",
        "Vue.js"
      ],
      "author": "Juan S",
      "license": "MIT"
    }


    Is this ok? (yes)

Al finalizar se habrá generado un archivo llamado **package.json** dentro del directorio, con el contenido descrito.

### 1.1.1 Instalación de paquetes con NPM

Una vez iniciado el proyecto procedemos a instalar los paquetes necesarios para el desarrollo de nuestro cliente:

Debemos agrupar los paquetes en 2 categorías:

* Paquetes que solo se utilizarán durante el desarrollo \(minificadores, transpiladores\), estos los instalaremos utilizando el comando: 

```
npm install [package_name] --save-dev
```

* Paquetes que el cliente requiere para funcionar, estos lo instalaremos utilizando:

```
npm install [package_name] --save
```

Que paquetes necesitamos para iniciar:

* Requeridos para el proyecto:
  * Vue \(vue\)
  * Vue Router \(vue-router\)
  * Axios \(axios\)
  * Lodash \(lodash\)
  * Jquery \(jquery\)
  * Twitter Bootstrap \(bootstrap\)
* Y para desarrollar:
  * Browserify \(browserify, browserify-shim, vueify, exorcist, babelify, uglifyify, watchify\)
  * File system methods \(fs-extra\)
  * Gulp \(gulp, vinyl-buffer, vinyl-source-stream, \)
  * Babel \(babel-core, babel-preset-env, babel-plugin-transform-runtime\)

Al terminar nuestro archivo **package.json** debe verse mas o menos asi \(los números de las versiones pueden ser diferentes\):

```json
{
  "name": "movies-api-client",
  "version": "0.0.1",
  "description": "Movies API Client",
  "main": "main.js",
  "scripts": {
    "test": "gulp test"
  },
  "keywords": [
    "Movies",
    "Vue.js"
  ],
  "author": "Juan S",
  "license": "MIT",
  "dependencies": {
    "axios": "^0.16.2",
    "jquery": "^3.2.1",
    "lodash": "^4.17.4",
    "vue": "^2.4.2",
    "vue-router": "^2.7.0"
  },
  "devDependencies": {
    "babel-core": "^6.26.0",
    "babel-plugin-transform-runtime": "^6.23.0",
    "babel-preset-env": "^1.6.0",
    "babelify": "^7.3.0",
    "browser-sync": "^2.18.13",
    "browserify": "^14.4.0",
    "browserify-shim": "^3.8.14",
    "fs-extra": "^4.0.1",
    "gulp": "^3.9.1",
    "uglifyify": "^4.0.3",
    "vinyl-buffer": "^1.0.0",
    "vinyl-source-stream": "^1.1.0",
    "vueify": "^9.4.1",
    "watchify": "^3.9.0"
  }
}
```

## Browserify

Browserify es un 

## Gulp

Gulp \([https://gulpjs.com](https://gulpjs.com)\) es un sistema de construcción basado en flujos, cuya principal utilidad es la automatización de las tareas.

Para utilizar Gulp además de los paquetes instalados anteriormente en el proyecto debemos instalar el paquete del cliente de linea de comandos de gulp, este paquete debe ser instaldo de manera global, esto lo hacemos con el siguiente comando:

```
npm install gulp-cli -g
```

Esta instalación solo debe ser realizada una vez y servirá para cualquier proyecto futuro.

Gulp requiere que las tareas sean definidas en un archivo llamado gulpfile.js

```

```



