# Hagamos un CRUD

Pare nuestro CRUD lo primero que necesitamos hacer e una clase repositorio que nos permita comunicarnos con la API:

```js
import axios from 'axios'

let api_url = 'http://localhost:5000/api/v1';

class PeliculasRepo {
    obtenerTodas() {
        return axios.get(`${ api_url }/peliculas`);
    }

    obtenerUna(id) {
        return axios.get(`${ api_url }/peliculas/${ id }`);
    }

    agregar(pelicula) {
        return axios.post(`${ api_url }/peliculas`, pelicula);
    }

    modificar(id, pelicula) {
        return axios.put(`${ api_url }/peliculas/${ id }`, pelicula);
    }

    eliminar(id,) {
        return axios.delete(`${ api_url }/peliculas/${ id }`);
    }
}

export default new PeliculasRepo();
```

Luego creamos nuestro componente:

```html
<template>
    <div>
        <h3>Películas</h3>
        <div class="row">
            <div class="col">
                <a href="" @click.prevent="agregar">Nueva película</a>
            </div>
        </div>
        <div class="row" v-if="showForm">
            <div class="col">
                <form @submit.prevent="guardar">
                    <div class="form-group">
                        <label for="pelicula_nombre">Nombre:</label>
                        <input type="text" class="form-control" id="pelicula_nombre" v-model="pelicula.nombre">
                    </div>
                    <div class="form-group">
                        <label for="pelicula_nombre">Código IMDB:</label>
                        <input type="text" class="form-control" id="pelicula_codigo" v-model="pelicula.codigoIMDB">
                    </div>
                    <div class="form-group">
                        <label for="pelicula_nombre">Código IMDB:</label>
                        <textarea class="form-control" id="pelicula_codigo" v-model="pelicula.resumen"></textarea>
                    </div>
                    <div class="form-group">
                        <button type="submit" class="btn btn-primary">Guardar</button>
                        <button type="button" class="btn btn-danger" @click="showForm = false">Cancelar</button>
                    </div>
                </form>
            </div>
        </div>
        <div class="row">
            <div class="col">
                <table class="table">
                    <thead>
                        <tr>
                            <th>#</th>
                            <th>Nombre</th>
                            <th>Codigo IMDB</th>
                            <th>Resumen</th>
                            <th></th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="pelicula, i in peliculas">
                            <td>{{ i + 1 }}</td>
                            <td>{{ pelicula.nombre }}</td>
                            <td>{{ pelicula.codigoIMDB }}</td>
                            <td>{{ pelicula.resumen }}</td>
                            <td>
                                <a href="" @click.prevent="editar(pelicula)">Editar</a>
                                <a href="" @click.prevent="eliminar(pelicula)">Eliminar</a>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
import peliculasRepo from '../repositories/peliculas-repo'

function obtenerPeliculas() {
    peliculasRepo.obtenerTodas().then(response => {
        this.peliculas = response.data;
    }).catch(error => {
        alert("Ocurrió un error descargando el listado de películas");
    });
}

function agregarPelicula() {
    peliculasRepo.agregar(this.pelicula).then(response => {
        obtenerPeliculas.call(this);
        this.showForm = false;
    }).catch(error => {
        alert("Ocurrió un error");
    });
}

function modificarPelicula() {
    peliculasRepo.modificar(this.id, this.pelicula).then(response => {
        obtenerPeliculas.call(this);
        this.id = null;
        this.showForm = false;
    }).catch(error => {
        alert("Ocurrió un error");
    });
}

function eliminarPelicula() {
    peliculasRepo.eliminar(this.id).then(response => {
        obtenerPeliculas.call(this);
        this.id = null;
    }).catch(error => {
        alert("Ocurrió un error");
    });
}

export default {
    data() {
        return {
            showForm: false,
            id: null,
            peliculas: [],
            pelicula: {}
        }
    },
    methods: {
        agregar() {
            this.showForm = true;
            this.id = null;
            this.pelicula = {};
        },
        editar(pelicula) {
            this.showForm = true;
            this.id = pelicula.id;
            this.pelicula = {
                nombre: pelicula.nombre,
                codigoIMDB: pelicula.codigoIMDB,
                resumen: pelicula.resumen,
                directorId: pelicula.directorId
            };
        },
        eliminar(pelicula) {
            if(confirm('Esta seguro??')) {
                this.id = pelicula.id;
                eliminarPelicula.call(this);
            }
        },
        guardar() {
            if(this.id) {
                modificarPelicula.call(this);
            } else {
                agregarPelicula.call(this);
            }
        }
    },
    created() {
        obtenerPeliculas.call(this);
    }
}
</script>
```

Las funciones \(Ej. obtenerPeliculas, agregarPelicula, modificarPelicula, eliminarPelicula\) pueden pasarse a otro archivo .js \(preferiblemente un servicio\), en caso de que el componente se vuelva muy pesado.

