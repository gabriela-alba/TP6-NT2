<template>

  <section class="src-componentes-formulario">

    <div class="jumbotron">
      <h1>Formulario</h1>
      <hr>
      <hr>
      <br>

      <vue-form :state="formState" @submit.prevent="enviar()">

        <!--CAMPO NOMBRE -->
        <validate tag="div">

          <label for="nombre">Nombre</label>
          <input 
            type="text"
            id="nombre" 
            name="nombre" 
            class="form-control"
            v-model.trim="formData.nombre"
            autocomplete="off"
            required
            :minlength="nombreMinLength"
            :maxlength="nombreMaxLength"
            no-espacios
          >

          <!--MENSAJE DE VALIDACIÓN NOMBRE -->
          <field-messages name="nombre" show="$dirty">

            <div slot="no-espacios" class="alert alert-danger mt-1">
              Este campo no permite espacios intermedios
            </div>

            <div slot="required" class="alert alert-danger mt-1">
              Campo requerido
            </div>

            <div slot="minlength" class="alert alert-danger mt-1">
              Este campo requiere como mínimo {{ nombreMinLength }} caracteres
            </div>

            <div v-if="formData.nombre.length == nombreMaxLength" class="alert alert-danger mt-1">
              Este campo requiere como máximo {{ nombreMaxLength }} caracteres
            </div>

          </field-messages>

        </validate>
        <br>

        <!--CAMPO EDAD -->
        <validate tag="div">

          <label for="edad">Edad</label>
          <input 
            type="number"
            id="edad" 
            name="edad" 
            class="form-control"
            v-model.number="formData.edad"
            autocomplete="off"
            required
            :min="edadMinLength"
            :max="edadMaxLength"
          >

          <!--MENSAJE DE VALIDACIÓN EDAD -->
          <field-messages name="edad" show="$dirty">

            <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>

            <div slot="min" class="alert alert-danger mt-1">
              La edad mínima permitida es de {{ edadMinLength }} años
            </div>
            
            <div slot="max" class="alert alert-danger mt-1">
              La edad máxima permitida es de {{ edadMaxLength }} años
            </div>

          </field-messages>

        </validate>
        <br>

        <!--CAMPO EMAIL -->
        <validate tag="div">

          <label for="email">Email</label>
          <input 
            type="email"
            id="email"
            name="email" 
            class="form-control"
            v-model.trim="formData.email"
            autocomplete="off"
            required
          >

          <!--MENSAJE DE VALIDACIÓN EMAIL -->
          <field-messages name="email" show="$dirty">

            <div slot="required" class="alert alert-danger mt-1">
              Campo requerido
            </div>

            <div slot="email" class="alert alert-danger mt-1">
              Email no válido
            </div>

          </field-messages>

        </validate>
        <br>

        <button class="btn btn-success my-3" :disabled="formState.$invalid">Enviar</button>

      </vue-form>

      <hr>
      <p><u>Datos</u></p>
      <pre>{{ datos }}</pre>

      <hr>
      <p><u>formData</u></p>
      <pre>{{ formData }}</pre>
      
      <p><u>formState</u></p>
      <pre>{{ formState }}</pre>

    </div>
  </section>

</template>

<script>

  export default  {
    name: 'src-componentes-formulario',
    props: [],

    /* ----------------------- LIFECYCLE HOOKS VUE ----------------------- */
    
    //Instanciación del componente
    beforeCreate() {
        console.log('Formulario -> Before create')
    },
    created() {
        console.log('Formulario -> Created')
    },
    beforeMount () {
        console.log('Formulario -> Before Mounted')
    },
    mounted() {
        console.log('Formulario -> Mounted')
        this.pedirDatosAlServidor()
    },
    data () {
      return {
        formData : this.getInicialData(), 
        formState : {}, // Objeto que indica con true o false si el campo está "Dirty"
        nombreMinLength : 5,
        nombreMaxLength : 15,
        edadMinLength : 18,
        edadMaxLength : 120,
        url : 'https://619488de9b1e780017ca1fcb.mockapi.io/post',
        //url : 'https://jsonplaceholder.typicode.com/post'
        datos : [],
      }
    },
    methods: {
      getInicialData() {
        return {
            nombre : '',
            edad : '',
            email : '',
          }
      },
      async enviarDatosAlServidor(datos) {
        try{
          let respuesta = await this.axios.post(this.url, datos, {'content-type':'application/json'})

          let datosRecibidos = respuesta.data
          console.log('datosRecibidos POST', datosRecibidos)

          //this.pedirDatosAlServidor()
          this.datos.push(datos)
        }
        catch(error) {
          console.error('Error en el envío de datos del formulario', error)
        }
      },
      async pedirDatosAlServidor() {
        try {
          let respuesta = await this.axios(this.url)

          let datos = respuesta.data
          console.log('datos GET', datos)

          this.datos = datos
        } catch (error) {
          console.error('Error en recepción de datos del servidor', error)
        }
      },
      enviar() {
        console.log('Enviando datos de usuario')
        
        console.log({...this.formData}) // SPREAD OPERATOR -> Muestra los datos en consola (clonar objeto)

        this.datos.push(this.datos)

        this.formData = this.getInicialData() //Reset de los valores
        this.formState._reset() //Reset de los estados
      }
    },
    computed: {

    }
}


</script>

<style scoped lang="css">

  .jumbotron {
    /* background-color: rgb(102, 5, 53);
    color: rgb(243, 225, 191); */
  
    /* Permalink - use to edit and share this gradient: https://colorzilla.com/gradient-editor/#efc5ca+0,d24b5a+14,ba2737+100,f18e99+100 */
    background: #efc5ca; /* Old browsers */
    background: -moz-linear-gradient(top,  #efc5ca 0%, #d24b5a 14%, #ba2737 100%, #f18e99 100%); /* FF3.6-15 */
    background: -webkit-linear-gradient(top,  #efc5ca 0%,#d24b5a 14%,#ba2737 100%,#f18e99 100%); /* Chrome10-25,Safari5.1-6 */
    background: linear-gradient(to bottom,  #efc5ca 0%,#d24b5a 14%,#ba2737 100%,#f18e99 100%); /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
    filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#efc5ca', endColorstr='#f18e99',GradientType=0 ); /* IE6-9 */



  }

  hr {
    background-color: #444;
  }
  
  p,
  pre {
    color: white;
  }
</style>
