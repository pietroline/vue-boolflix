<template>
  <div id="app">
    
    <MyHeader @ricerca="richiestaAPI"/>

    <MyMain :films="apiRisposta"/>
  </div>
</template>

<script>

  import MyHeader from "./components/MyHeader";
  import MyMain from "./components/MyMain";

  export default {
    name: 'App',
    components: {
      MyHeader,
      MyMain,
    },
    data(){
      return{
        apiKey: "645226498552dd0739079c2786443593",
        apiLanguage: "it-IT",
        apiRisposta: [],
      }
    },
    methods:{
      richiestaAPI(valoreCercato){

          //genero endPoint
          let endPoint = `https://api.themoviedb.org/3/search/movie?language=${this.apiLanguage}&api_key=${this.apiKey}&query=${valoreCercato}`;
          
          //effettuo richiesta axios
          const axios = require('axios');

          axios.get(endPoint)
            .then(response => {
              this.apiRisposta = response.data.results;
            })
            .catch(function (error) {
              console.log(error);
            })
            
      

          
        }
    }
  }
</script>

<style lang="scss">

</style>
