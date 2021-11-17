<template>

  <section class="src-componentes-usuarios">
    <div class="jumbotron">
      <h2>Componente de Solicitud recurso mockAPI</h2>
      <hr>
      <hr>
      <br>

      <button class="btn btn-primary my-3 mr-3" @click="getPostAxios()">Pedir Axios</button>

      <h4 v-if="datos.length === 0 && !peticion" class="alert alert-secondary">No se encontraron usuarios registrados</h4>
      
      <!-- <pre>{{ datos }}</pre> -->
      <div v-if="datos.length" class="table-responsive">
        <table class="table table-dark">
          <tr>
            <th>Nombre</th>
            <th>Edad</th>
            <th>Email</th>
          </tr>
          <tr v-for="(dato, index) in datos" :key="index">
            <td>{{ dato.nombre }}</td>
            <td>{{ dato.edad }}</td>
            <td>{{ dato.email }}</td>
          </tr>
        </table>
        <h4 class="alert alert-primary">Se encontraron {{datos.length}} registros</h4>
      </div>
      <h4 v-else-if="peticion" class="alert alert-danger">Cargando datos...</h4>  

    </div> 

  </section>

</template>

<script>
  export default  {
    name: 'src-componentes-usuarios',
    props: [],
    mounted () {

    },
    data () {
      return {
        url : 'https://619488de9b1e780017ca1fcb.mockapi.io/post',   // Prueba para muestreo en tabla datos posts
        //url: 'https://61902556f6bf450017484b6f.mockapi.io/datos',
        datos : [],
        peticion: false
      }
    },
    methods: {

      /* ---------- AJAX: Axios ----------- */
      async getPostAxios() {
        this.datos = []
        this.peticion = true         
        /* Con sintaxis Then/catch */
        // this.axios(this.url)
        // .then(respuesta => {
        //   console.log('AXIOS', respuesta.data)
          // this.datos = respuesta.data

        // })
        // .catch(error => console.error('Error AXIOS', error))

        try {
          let respuesta = await this.axios(this.url)
          console.log('AXIOS', respuesta.data)
          this.datos = respuesta.data
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
      /* Permalink - use to edit and share this gradient: https://colorzilla.com/gradient-editor/#feccb1+0,f17432+27,ea5507+61,ea5507+61,fb955e+100 */
      background: #feccb1; /* Old browsers */
      background: -moz-linear-gradient(left,  #feccb1 0%, #f17432 27%, #ea5507 61%, #ea5507 61%, #fb955e 100%); /* FF3.6-15 */
      background: -webkit-linear-gradient(left,  #feccb1 0%,#f17432 27%,#ea5507 61%,#ea5507 61%,#fb955e 100%); /* Chrome10-25,Safari5.1-6 */
      background: linear-gradient(to right,  #feccb1 0%,#f17432 27%,#ea5507 61%,#ea5507 61%,#fb955e 100%); /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
      filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#feccb1', endColorstr='#fb955e',GradientType=1 ); /* IE6-9 */
  }
  hr {
    background-color: #444;
  }

</style>
