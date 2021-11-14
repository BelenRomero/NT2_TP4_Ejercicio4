<template lang="html">

  <section class="src-componentes-formulario">
    <div class="jumbotron">
      <h2>Formulario ingreso de datos</h2>
      <hr>

      <vue-form :state="formstate" @submit.prevent="enviar()">
        
        <!-- Campo Nombre -->
        <validate tag="div">
          <label for="nombre">Nombre</label>
          <input type="text" id="nombre" v-model.trim="formData.nombre" required minlength="5" maxlength="15" name="nombre" autocomplete="off" class="form-control" />
    
          <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
            <div slot="minlength" class="alert alert-danger mt-1">Este campo debe poseer al menos 5 caracteres</div>
            <div v-if="formData.nombre && (formData.nombre.length == 15)" class="alert alert-danger mt-1">Este campo debe poseer como máximo 15 caracteres</div>
          </field-messages>
        </validate>
        <br>
        
        <!-- Campo Edad -->
        <validate tag="div">
          <label for="edad">Edad</label>
          <input type="number" id="edad" v-model.number="formData.edad" required name="edad" autocomplete="off" class="form-control" />
    
          <field-messages name="edad" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
            <div v-if="formData.edad && (formData.edad < 18)" class="alert alert-danger mt-1">La edad mínima admitida es de 18 años</div>
            <div v-if="formData.edad && (formData.edad > 120)" class="alert alert-danger mt-1">La edad máxima admitida es de 120 años</div>
          </field-messages>
        </validate>
        <br>

        <!-- Campo Email -->
        <validate tag="div">
          <label for="email">Email</label>
          <input type="email" id="email" v-model="formData.email" required name="email" autocomplete="off" class="form-control" input="formDirty.email=true" />
    
          <field-messages name="email" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
          </field-messages>
        </validate>
        
        <br>
        
        <button class="btn btn-success my-3" :disabled="formstate.$invalid" type="submit">Enviar</button>
      </vue-form>
      <br>
      <hr>

      <!-- Tabla para representar los datos ingresados -->
      <h2>Detalle datos ingresados</h2>
      <hr>

      <!-- <pre>{{ datos }}</pre> -->

      <div v-if="datos.length" class="table-responsive">
        <table class="table">
          <tr>
            <th>Nombre</th>
            <th>Edad</th>
            <th>Email</th>
          </tr>
          <tr v-for="(dato,index) in datos" :key="index">
            <td>{{ dato.nombre }}</td>
            <td>{{ dato.edad }}</td>
            <td>${{ dato.email }}</td>
          </tr>
        </table>
      </div>
      <h3 v-else class="alert alert-danger">No hay datos ingresados</h3>

    </div>
  </section>

</template>





<script>

  export default  {
    name: 'src-componentes-formulario',
    props: [],
    mounted () {

    },

    data () {
      return {
        formstate : {},
        formData : this.getInitialData(),
        datos : [],
      }
    },

    methods: {
      getInitialData() {
        return {
          nombre : null,
          edad: null,
          email: null
        }
      },

      enviar() {
        let dato = {...this.formData}

        //console.log(dato)
        this.datos.push(dato)

        this.formData = this.getInitialData()
        this.formstate._reset()
      }
      
    },
    computed: {

    }
  }

</script>






<style scoped lang="css">

  .src-componentes-formulario {

  }

    .jumbotron {
    background-color: lightgray;
    color: black;
  }

</style>
