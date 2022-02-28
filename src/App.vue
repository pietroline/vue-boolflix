<template>
  <div id="app">
    
    <MyHeader @ricerca="richiestaAPI"/>

    <MyMain :films="apiRispostaFilms" :series="apiRispostaSerieTv" :valoreCercato="valoreCercato"/>
  </div>
</template>

<script>

  import MyHeader from "./components/MyHeader";
  import MyMain from "./components/MyMain";

  //importo axios
  const axios = require('axios');

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
        apiRispostaFilms: [],
        apiRispostaSerieTv: [],
        valoreCercato: null,
      }
    },
    methods:{

      richiestaAPI(valoreCercato){

        this.valoreCercato = valoreCercato;

        //variabile contenente la sezione di ricerca su TMDB
        const ricerche = ["movie", "tv"];
        
        ricerche.forEach(element => {

          //genero endPoint
          let endPoint = `https://api.themoviedb.org/3/search/${element}?language=${this.apiLanguage}&api_key=${this.apiKey}&query=${valoreCercato}`;
          
          //effettuo richiesta axios
          axios
              .get(endPoint)

              .then(response => {
                if(element == ricerche[0]){
                  this.apiRispostaFilms = response.data.results;
                }else if(element == ricerche[1]){
                  this.apiRispostaSerieTv = response.data.results;
                }
              })

              .catch(function (error) {
                console.log(error);
              });
                
        });        

      }

    }
  }
</script>

<style lang="scss">

</style>
