# Trabajando con datos externos

## 4.1. Axios \([https://github.com/mzabriskie/axios](https://github.com/mzabriskie/axios)\)

Axios es un cliente HTTP para node.js y navegadores, con soporte para la API [Promise](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Promise).  Es una librería simple y altamente configurable, inspirada en el servicio $http de angular.

Utilizaremos axios para consumir nuestra API web \([https://jsnoriegam.gitbooks.io/introduccion-al-desarrollo-de-apis-web-con-net-co/content/](https://jsnoriegam.gitbooks.io/introduccion-al-desarrollo-de-apis-web-con-net-co/content/)\)

### 4.1.1 Métodos \(API de Axios\)

**axios\(config\)**

```js
axios({
  method: 'put',
  url: '/user/12345',
  data: {
    firstName: 'Pedro',
    lastName: 'Picapiedra'
  }
});
```

Métodos abreviados

```js
axios.request(config)

axios.get(url[, config])

axios.delete(url[, config])

axios.head(url[, config])

axios.options(url[, config])

axios.post(url[, data[, config]])

axios.put(url[, data[, config]])

axios.patch(url[, data[, config]])
```

Para mas información sobre la API de axios ir a: \([https://github.com/mzabriskie/axios/blob/master/README.md](https://github.com/mzabriskie/axios/blob/master/README.md)\)

### 4.1.2 Promesas

Las llamadas a axios retornan promesas \([https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos\_globales/Promise](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Promise)\) por lo tanto para obtener los resultados de una petición debemos utiliar los métodos establecidos por dicha API ej.

```js
axios.post('/user', {
  firstName: 'Pedro',
  lastName: 'Picapiedra'
})
.then(function (response) {
  console.log(response);
})
.catch(function (error) {
  console.log(error);
});
```

```js
axios.get('/user/12345')
.catch((error) => {
  if (error.response) {
    // The request was made and the server responded with a status code
    // that falls out of the range of 2xx
    console.log(error.response.data);
    console.log(error.response.status);
    console.log(error.response.headers);
  } else if (error.request) {
    // The request was made but no response was received
    // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
    // http.ClientRequest in node.js
    console.log(error.request);
  } else {
    // Something happened in setting up the request that triggered an Error
    console.log('Error', error.message);
  }
  console.log(error.config);
});
```

## 4.2. Autenticación



