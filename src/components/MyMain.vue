<template>
    <main class="container-fluid">
        <ul class="text-white mt-5">
            <div v-if="films.length > 0">
                <li class="mb-5 list-unstyled" v-for="(film, index) in films" :key="index">
                    <span>Film n° {{index+1}}:</span>
                    <ul>
                        <li>Titolo: {{film.title}} </li>
                        <li v-show="film.title!=film.original_title">Titolo Originale: {{film.original_title}} </li>
                        <li>Lingua: {{emojiFlag(index)}}</li>
                        <li>Voto: {{film.vote_average}}</li>
                    </ul>
                </li>
            </div>
            <div v-else-if="valoreCercato !=null">
                <div>
                    <span class="fs-1">(Non ci sono risultati)</span>
                    <div>Hai cercato: "{{valoreCercato}}"</div>
                </div>
            </div>
            <div v-else class="text-center fs-1">
                <div class="m-5">Benvenuto in Boolflix</div>
                <div class="m-5">Adesso puoi guardare film e serie tv preferite stando comodamente seduto sul tuo divano</div>
                <div class="m-5">Cosa aspetti inizia subito una ricerca!!!</div>
            </div>
        </ul>
    </main>
</template>

<script>
    export default {
        name: "MyMain",
        props:{
            "films": Array,
            "valoreCercato": String,
        },
        methods:{
            emojiFlag(indice){
                //uesto metodo serve a generare l'emoticon della bandiera adatta in base al dato ricevuto dal server API

                let emoji = require('node-emoji')
                let emoticon = this.films[indice].original_language;

               
                if(emoticon == "en"){ //gestione lingua inglese
                    emoticon = "gb";
                }else if(emoticon == "hi"){ //gestione lingua hindi
                    emoticon = "in";
                }else if(emoticon == "sv"){ //gestione lingua svedese
                    emoticon = "ax";
                }else if(emoticon == "ja"){ //gestione lingua giapponese
                    emoticon = "jp";
                }else if(emoticon == "ko"){ //gestione lingua coreana
                    emoticon = "kr";
                }
                //...eventualmente gestire altre lingue...

                return emoji.get(`flag-${emoticon}`)
            }
        }
    }
</script>

<style lang="scss" scoped>

    @import "./../assets/variables.scss";

    .container-fluid{
        height: calc(100vh - 4rem);
        background-color: $colorMain;

        overflow: scroll;
    }
</style>