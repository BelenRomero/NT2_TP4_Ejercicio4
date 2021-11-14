<template>

  <section class="src-componentes-componente">
    <div class="jumbotron">
      <h2>Componente de Solicitud recurso mockAPI</h2>
      <hr>
      <hr>
      <br>

      <button class="btn btn-danger my-3 mr-3" @click="getPostCb()">Pedir XHR (CB)</button>
      <button class="btn btn-danger my-3 mr-3" @click="getPostPromise()">Pedir XHR (Promise)</button>
      <button class="btn btn-warning my-3 mr-3" @click="getPostFetch()">Pedir fetch</button>
      <button class="btn btn-primary my-3 mr-3" @click="getPostAxios()">Pedir Axios</button>

      <!-- <pre>{{ usuarios }}</pre> -->
      <div v-if="usuarios.length" class="table-responsive">
        <table class="table table-dark">
          <tr>
            <th>ID</th>
            <th>Nombre</th>
            <th>Email</th>
            <th>Telefono</th>
          </tr>
          <tr v-for="(usuario, index) in usuarios" :key="index">
            <td>{{ usuario.id}}</td>
            <td>{{ usuario.nombre}}</td>
            <td>{{ usuario.email}}</td>
            <td>{{ usuario.telefono}}</td>
          </tr>
        </table>
        <h4 class="alert alert-primary">Se encontraron {{usuarios.length}} registros</h4>
      </div>
      <h4 v-else-if="peticion" class="alert alert-danger">Cargando datos...</h4>  

    </div>

  </section>

</template>





<script>

  export default  {
    name: 'src-componentes-componente',
    props: [],
    mounted () {

    },
    data () {
      return {
        //url: 'https://61902556f6bf450017484b6f.mockapi.io/posts',   // Prueba para muestreo en tabla datos posts
        url: 'https://61902556f6bf450017484b6f.mockapi.io/usuarios',
        usuarios : [],
        peticion: false
      }
    },
    methods: {
      /* --------------------------------------------------------------------------- */
      /*                   AJAX : Asynchronous Javascript And XML                    */
      /* --------------------------------------------------------------------------- */

      /* ---------- AJAX: XMLHttpRequest ----------- */
      getPostCb() {
        this.usuarios = []
        this.peticion = true

        /* Creo una instancia de comunicación asincrónica */
        let xhr = new XMLHttpRequest()
        /* Configura la instancia */
        xhr.open('get',this.url)
        /* Registramos el evento de fin de la comunicación */
        xhr.addEventListener('load', () => {
          /* Comunicación exitosa */
          if(xhr.status == 200) {
            let respuesta = JSON.parse(xhr.response)
            //console.log(typeof respuesta)
            console.log('XHR CB', respuesta)
            this.usuarios = respuesta
            this.peticion = false
          }
          /* Comunicación error */
          else {
            console.error(`ERROR XHR CB (status): ${xhr.status}`)
          }
        })
        /* Registramos el evento de error de comunicación */
        xhr.addEventListener('error', e => {
            console.error('ERROR XHR CB (event):', e)
        })
        xhr.send()
      },

      postPromise() {
        return new Promise((resolve,reject) => {
          /* Creo una instancia de comunicación asincrónica */
          let xhr = new XMLHttpRequest()
          /* Configura la instancia */
          xhr.open('get',this.url)
          /* Registramos el evento de fin de la comunicación */
          xhr.addEventListener('load', () => {
            /* Comunicación exitosa */
            if(xhr.status == 200) {
              let respuesta = JSON.parse(xhr.response)
              //console.log(typeof respuesta)
              //console.log(respuesta)
              resolve(respuesta)
            }
            /* Comunicación error */
            else {
              let error = {
                title: 'Error de status',
                status: xhr.status
              }
              reject(error)              
            }
          })
          /* Registramos el evento de error de comunicación */
          xhr.addEventListener('error', e => {
              let error = {
                title: 'Error event XHR',
                info: e
              }
              reject(error)  
          })

          xhr.send()
        })
      },
      async getPostPromise() {
        this.usuarios = []
        this.peticion = true

        /* Promesas con Async/Await */
        try {
          let respuesta = await this.postPromise()
          console.log('XHR Promise', respuesta)
          this.usuarios = respuesta
          this.peticion = false
        }
        catch(error) {
          console.error('Error XHR Promise', error)
        }
      },

      /* ---------- AJAX: fetch ----------- */
      async getPostFetch() {
        this.usuarios = []
        this.peticion = true

        /* Con sintaxis Async/Await */
        try {
          let response = await fetch(this.url)
          console.log(response)
          let respuesta = await response.json()
          console.log('FETCH', respuesta)
          this.usuarios = respuesta
          this.peticion = false
        }
        catch(error) {
          console.error('Error FETCH', error)
        }
      },

      /* ---------- AJAX: Axios ----------- */
      async getPostAxios() {
        this.usuarios = []
        this.peticion = true         

        /* Con sintaxis Then/catch */
        try {
          let respuesta = await this.axios(this.url)
          console.log('AXIOS', respuesta.data)
          this.usuarios = respuesta.data
          this.peticion = false
        }
        catch(error) {
          console.error('Error AXIOS', error)
        }
      }
    },
    computed: {

    }
  }

</script>





<style scoped lang="css">
  .src-componentes-componente {

  }

    .jumbotron {
    background-color: lightgray;
    color: black;
  }

  hr {
    background-color: #fff;
  }

  pre {
    color: white;
  }
</style>
