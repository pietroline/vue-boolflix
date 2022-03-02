<template>
  <div id="app">
    
    <MyHeader @ricerca="richiestaAPI" :allGenresFilms="allGenresFilms" @genreSelectedFilms="genreSelectedFilms"/>

    <MyMain :films="apiRispostaFilmsFiltrata" :series="apiRispostaSerieTv" :valoreCercato="valoreCercato"/>
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
    mounted: function () {
      this.$nextTick(function () {

        // richiesta per tutti i generi film
        axios
              .get(`https://api.themoviedb.org/3/genre/movie/list?api_key=${this.apiKey}&language=en-US`)

              .then(response => {
                this.allGenresFilms=response.data.genres;
              })

              .catch(function (error) {
                console.log(error);
              });

      })

    },
    data(){
      return{
        apiKey: "645226498552dd0739079c2786443593",
        apiLanguage: "it-IT",
        apiAppendToResponse: "credits",
        apiRispostaFilms: [],
        apiRispostaSerieTv: [],
        valoreCercato: null,
        allGenresFilms: [],
        genereSelezionatoFilms: "",
        allGenresSeriesTv: [],
        genereSelezionatoSerieTv: "",
      }
    },
    computed: {
      apiRispostaFilmsFiltrata(){
        let ritorno = [];
        if(this.genereSelezionatoFilms != ""){
          
            ritorno = this.apiRispostaFilms.filter(film =>{
              for(let i=0; i<film.generi.length; i++){
                if(film.generi[i] == this.genereSelezionatoFilms){
                  return film;
                }
              }
            });
          
        }else{
          ritorno = this.apiRispostaFilms;
        }

        return ritorno;
      },

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
                  this.getDetailsSerieTv();
                }
              })

              .catch(function (error) {
                console.log(error);
              });
                
        });        

      },

      
      getDetailsFilms() {
        //questo metodo viene invocato dopo aver popolato apiRIspostaFilms con la richiesta axios

        //creo un ciclo con un numero di iterazioni pari alla dimensione di apiRispostaFilms
        this.apiRispostaFilms.forEach((film, index) => {
          
          //creo una variabile in cui "salvare l'informazione sui generi e sul cast del film preso in esame"
          let generi = [];
          let cast = [];

          //efettuo nuova richiesta axios, con chiamata a GET movie/movieID come da documentazione theMovieDB
          axios
            .get(`https://api.themoviedb.org/3/movie/${film.id}?api_key=${this.apiKey}&append_to_response=${this.apiAppendToResponse}`)

              .then((response) => {

                if(response.data.genres.length > 0){
                  //genero un nuovo ciclo ripetuto un numero di volte pari al numero di generi del film presi dalla risposta axios appena effetuata
                  response.data.genres.forEach((genre, index) => {

                    //popolo la variabile generi, precedentemente creata, con tutti i generi ritornati come risposta API del film in posizione index
                    generi[index] = genre.name;
                  });
                }else{
                  generi = null;
                }
                

                //genero un nuovo ciclo ripetuto 5 volte al fine di prelevare le informazioni sul cast del film prese dalla risposta axios appena effetuata
                if(response.data.credits.cast.length > 0){
                  for (let i = 0; i < 5; i++) {
                    cast[i] = response.data.credits.cast[i];
                  }
                }else{
                  cast = null;
                }
                

                //aggiungo la nuova proprietà generi a apiRispostaFilms in maniera reactive
                this.$set(this.apiRispostaFilms[index], "generi", generi);

                //aggiungo la nuova proprietà cast a apiRispostaFilms in maniera reactive
                this.$set(this.apiRispostaFilms[index], "cast", cast);
                  
              })

              .catch(function (error) {
                console.log(error +` sul film con id: ${film.id} (nome film: ${film.title}) non ci sono ulteriori dettagli sul film`);
              });
        });
      },

      getDetailsSerieTv() {
        //questo metodo viene invocato dopo aver popolato apiRIspostaFilms con la richiesta axios

        //creo un ciclo con un numero di iterazioni pari alla dimensione di apiRispostaFilms
        this.apiRispostaSerieTv.forEach((serie, index) => {
          
          //creo una variabile in cui "salvare l'informazione sui generi e sul cast del film preso in esame"
          let generi = [];
          let cast = [];

          //efettuo nuova richiesta axios, con chiamata a GET movie/movieID come da documentazione theMovieDB
          axios
            .get(`https://api.themoviedb.org/3/movie/${serie.id}?api_key=${this.apiKey}&append_to_response=${this.apiAppendToResponse}`)

              .then((response) => {

                if(response.data.genres.length > 0){
                  //genero un nuovo ciclo ripetuto un numero di volte pari al numero di generi del film presi dalla risposta axios appena effetuata
                  response.data.genres.forEach((genre, index) => {

                    //popolo la variabile generi, precedentemente creata, con tutti i generi ritornati come risposta API del film in posizione index
                    generi[index] = genre.name;
                  });
                }else{
                  generi = null;
                }
                
                //genero un nuovo ciclo ripetuto 5 volte al fine di prelevare le informazioni sul cast del film prese dalla risposta axios appena effetuata
                if(response.data.credits.cast.length > 0){
                  for (let i = 0; i < 5; i++) {
                    cast[i] = response.data.credits.cast[i];
                  }
                }else{
                  cast = null;
                }
                

                //aggiungo la nuova proprietà generi a apiRispostaFilms in maniera reactive
                this.$set(this.apiRispostaSerieTv[index], "generi", generi);

                //aggiungo la nuova proprietà cast a apiRispostaFilms in maniera reactive
                this.$set(this.apiRispostaSerieTv[index], "cast", cast);
                  
              })

              .catch(function (error) {
                console.log(error +` sul serie tv con id: ${serie.id} (nome serie tv: ${serie.name}) non ci sono ulteriori dettagli sulla serie tv`);
              });
        });
      },

      genreSelectedFilms(genereSelezionatoFilms){
        this.genereSelezionatoFilms = genereSelezionatoFilms;
      },

    }
  }
</script>

<style lang="scss">

</style>
