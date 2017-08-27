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
        <div id="app">
            <router-view></router-view>
        </div>
        <script src="dist/app.js"></script>
    </body>
</html>
```

Puesto que vamos a utilizar la estructura base propuesta por el gulpfile.js, como segundo paso debemos crear un directorio llamado src y un archivo llamado main.js



