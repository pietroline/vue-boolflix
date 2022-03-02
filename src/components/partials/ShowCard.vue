<template>
    <div class="card-flip">
        
        <div class="card-front">
            <img v-if="poster_path == null" src="./../../assets/img/noPhotoAvailable.png" alt="">
            <img v-else :src="'https://image.tmdb.org/t/p/w342'+poster_path" :alt="'Img ' + titolo_originale">
        </div>
        
        <div class="card-back bg-dark overflow-scroll">
            <div class="card-body">
 
                <div class="py-2">
                    <span>Titolo:&nbsp;</span> 
                    <h1 class="fs-6 d-inline fw-normal">{{titolo}}</h1> 
                </div>
  
                <div class="py-2" v-show="titolo!=titolo_originale">
                    <span>Titolo Originale:&nbsp;</span>
                    <h1 class="fs-6 d-inline fw-normal">{{titolo_originale}}</h1>
                </div>

                <div class="py-2" v-if="genres != null">
                    <span>Genere:&nbsp;</span>
                    <h5 class="fs-6 d-inline">
                       
                        <span class="fw-normal" 
                                v-for="i in genres.length" 
                                :key="'genere '+i">
                        
                            <!-- inizio contenuto span -->
                            {{genres[i-1]}} <span v-show="i != genres.length">,</span> &nbsp;
                            <!-- fine contenuto span -->
                        </span>
 
                    </h5>
                </div>
   
                <div class="py-2" v-if="cast != null">
                    <span>Attori:&nbsp;</span>
                    <h5 class="fs-6 d-inline">
 
                        <span class="fw-normal" 
                                v-for="i in cast.length" 
                                :key="'cast '+i">

                            <!-- inizio contenuto span -->
                            {{cast[i-1].original_name}} <span v-show="i != cast.length">,</span> &nbsp;
                            <!-- fine contenuto span -->
                        </span>
 
                    </h5>
                </div>

                <div class="py-2">
                    <span>Lingua:&nbsp;</span>
                    <h4 class="fs-6 d-inline fw-normal">{{emojiFlag(lingua_originale)}}</h4>
                </div>

                <div class="py-2">
                    <span>Voto:&nbsp;</span> 

                    <!-- stelle piene -->
                    <span v-for="i in getStars().piene" :key="i">
                        <i class="bi bi-star-fill text-warning"></i>
                    </span>

                    <!-- stella mezza -->
                    <span v-show="getStars().piene + getStars().vuote < 5">
                        <i class="bi bi-star-half text-warning"></i>
                    </span>

                    <!-- stelle vuote -->
                    <span v-for="i in getStars().vuote" :key="i+getStars().piene">
                        <i class="bi bi-star"></i>
                    </span>

                    <span>&nbsp;(<u class="voteCount">{{vote_count}}</u>)</span>
                </div>

                <div class="py-2" v-show="overview">
                    <span>Overview:&nbsp;</span> 
                    <p class="d-inline">{{overview}}</p> 
                </div>
            </div>
        </div>
        
    </div>     
</template>

<script>

    //importo node-emoji, nessessario per emojiFlag(language)
    let emoji = require('node-emoji');

    // importo mathjs, necessario per scoreVote()
    let math = require("mathjs");

    export default {
        name: "ShowCard",
        props:{
            "titolo": String,
            "titolo_originale": String,
            "lingua_originale": String,
            "voto": Number,
            "poster_path": String,
            "overview": String,
            "vote_count": Number,
            "genres": Array,
            "cast": Array,
        },
        methods:{   

            testGeneri(generi){
                console.log(generi);
            },
            // testAttori(attoriFilm){
            //     console.log(attoriFilm);
            // },

            getStars(){
                // converto range voto 0-10 in range voto 0-5, e calcolo voto stellePiene e stelleVuote
                const voto = math.round(this.voto);
                const stellePiene = math.floor(voto/2);
                let stelleVuote = 5 - stellePiene;

                //calcolo la presenza della stella mezza
                const resto = (voto/2 - stellePiene);
                if(resto == 0.5){
                    stelleVuote -= 1;
                }

                const stelle = {
                    "piene": stellePiene,
                    "vuote": stelleVuote,
                }
                                
                return stelle;
            },

            emojiFlag(language){
                //questo metodo serve a generare l'emoticon della bandiera adatta in base al dato ricevuto dal server API

                let emoticon = language;

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

    @import "./../../assets/variables.scss";

    img{
        /* imposto altezza fissa, la larghezza è uguale per tutte le img ed è data dal server api,
        per come sono inseriti gli src => w342 */
        height: 35rem;
        width: 25rem; /*forzo anche larghezza fissa, diversa da w342 ma vicina per dimensione, così la img è uguale in dimensioni alla card-flip*/
    }

    .card-flip > div {
        backface-visibility: hidden;
        transition: transform 300ms linear;
        height: 35rem;
        width: 25rem;
    }

    .card-flip{
        position: relative;
        cursor: pointer;
    }

    .card-back {
        transform: rotateY(180deg);
        position: absolute;
        top: 0;
    }

    .card-flip:hover .card-front {
        transform: rotateY(-180deg);
    }
  
    .card-flip:hover .card-back {
        transform: rotateY(0deg);
    }

    .card-body div span:first-child{
        font-weight: bold;
    }

    .voteCount:hover{
        color:$colorTextHover;
    }

    .noImg{
        width: 373px;
        height: 35rem;
        background-color: $colorTextHover;
    }
</style>