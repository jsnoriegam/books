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

* Paquetes que solo se utilizarán durante el desarrollo \(minificadores, transpiladores\)
* Paquetes que el cliente requiere para funcionar

## Gulp



