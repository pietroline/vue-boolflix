<template>

    <ul>
        <li>Titolo: {{titolo}} </li>
        <li v-show="titolo!=titolo_originale">Titolo Originale: {{titolo_originale}} </li>
        <li>Lingua: {{emojiFlag(lingua_originale)}}</li>
        <li>Voto: {{voto}}</li>
    </ul>
            
</template>

<script>

    //importo node-emoji, nessessario per emojiFlag(language)
    let emoji = require('node-emoji');

    export default {
        name: "ShowCard",
        props:{
            "titolo": String,
            "titolo_originale": String,
            "lingua_originale": String,
            "voto": Number,
        },
        methods:{   

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