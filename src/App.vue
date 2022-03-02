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
        apiAppendToResponse: "credits",
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
                  this.getDetailsFilms();
                }else if(element == ricerche[1]){
                  this.apiRispostaSerieTv = response.data.results;
                  //implementare getDetailsSerieTv
                }
              })

              .catch(function (error) {
                console.log(error);
              });
                
        });        

      },

      getDetailsFilms(){
        //questo metodo viene invocato dopo aver popolato apiRIspostaFilms con la richiesta axios

        //creo un ciclo con un numero di iterazioni pari alla dimensione di apiRispostaFilms 
        this.apiRispostaFilms.forEach((film) =>{

          //ad ogni oggetto di apiRispostaFilms aggiungo una nuova proprietà per "salvare" l'informazione sui generi e sul cast del film preso in esame 
          film.generi=[];
          film.cast=[];

          //efettuo nuova richiesta axios, con chiamata a GET movie/movieID come da documentazione theMovieDB
          axios
              .get(`https://api.themoviedb.org/3/movie/${film.id}?api_key=${this.apiKey}&append_to_response=${this.apiAppendToResponse}`)

              .then(response => {

                //genero un nuovo ciclo ripetuto un numero di volte pari al numero di generi del film presi dalla risposta axios appena effetuata
                response.data.genres.forEach((genre, index) =>{

                  //popolo la proprietà generi, precedentemente creata, con tutti i generi ritornati come risposta API del film in posizione index
                  film.generi[index] = genre.name;
                })

                //genero un nuovo ciclo ripetuto 5 volte al fine di prelevare le informazioni sul cast del film prese dalla risposta axios appena effetuata
                for(let i=0; i<5; i++){
                  film.cast[i] = response.data.credits.cast[i];
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
