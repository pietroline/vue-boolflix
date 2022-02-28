<template>
    
    <ul>
        <li class="list-unstyled"><img :src="'https://image.tmdb.org/t/p/w342'+poster_path" :alt="'Img ' + titolo_originale"></li>
        <li>Titolo: {{titolo}} </li>
        <li v-show="titolo!=titolo_originale">Titolo Originale: {{titolo_originale}} </li>
        <li>Lingua: {{emojiFlag(lingua_originale)}}</li>
        <li>
            <span>Voto:</span> 

            <!-- stelle piene -->
            <span v-for="i in getStars().piene" :key="i">
                <i class="bi bi-star-fill"></i>
            </span>

             <!-- stella mezza -->
            <span v-show="getStars().piene + getStars().vuote < 5">
                <i class="bi bi-star-half"></i>
            </span>

            <!-- stelle vuote -->
            <span v-for="i in getStars().vuote" :key="i+getStars().piene">
                <i class="bi bi-star"></i>
            </span>
        </li>
    </ul>
            
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
        },
        methods:{   

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

<style>

</style>