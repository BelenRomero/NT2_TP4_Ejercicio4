<template>

  <section class="src-componentes-componente">
    <div class="jumbotron">
      <h2>Componente de Solicitud recurso mockAPI</h2>
      <hr>
      <hr>
      <br>

      <button class="btn btn-danger my-3 mr-3" @click="getPostCb()">Pedir XHR (cb)</button>
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
            <td>{{ usuarios.id}}</td>
            <td>{{ usuarios.nombre}}</td>
            <td>{{ usuarios.email}}</td>
            <td>{{ usuarios.telefono}}</td>
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
            console.log('XHR Cb respuesta', respuesta)
            this.usuarios = respuesta
            console.log('XHR Cb this.usuarios', respuesta)
            this.peticion = false
          }
          /* Comunicación error */
          else {
            console.error(`ERROR XHR Cb (status): ${xhr.status}`)
          }
        })
        /* Registramos el evento de error de comunicación */
        xhr.addEventListener('error', e => {
            console.error('ERROR XHR Cb (event):', e)
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

        /* Con sintaxis Async/Await  */
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
      /* Permalink - use to edit and share this gradient: https://colorzilla.com/gradient-editor/#7d7e7d+0,0e0e0e+100;Black+3D */
      background: #7d7e7d; /* Old browsers */
      background: -moz-linear-gradient(left,  #7d7e7d 0%, #0e0e0e 100%); /* FF3.6-15 */
      background: -webkit-linear-gradient(left,  #7d7e7d 0%,#0e0e0e 100%); /* Chrome10-25,Safari5.1-6 */
      background: linear-gradient(to right,  #7d7e7d 0%,#0e0e0e 100%); /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
      filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#7d7e7d', endColorstr='#0e0e0e',GradientType=1 ); /* IE6-9 */

      color: white;
  }

</style>
