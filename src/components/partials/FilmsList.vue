<template>
    <section>

        
        <h1 class="categoria" v-show="films.length > 0"  @click="mostraNascondi()">FILM</h1>
    
        <ul v-if="films.length > 0" class="row" v-show="filmsCercati">

            <li class="col mb-5 list-unstyled" 
                v-for="(film, index) in films" 
                :key="index">

            <ShowCard 
                :titolo="film.title" 
                :titolo_originale="film.original_title" 
                :lingua_originale="film.original_language" 
                :voto="film.vote_average"
                :poster_path="film.poster_path"
                :overview="film.overview"
                :vote_count="film.vote_count"/>
    
            </li>

        </ul>
        <div v-else>
            <!-- delay() serve per gestire il primissimo caso, dopo aver lanciato la pagina web -->
            <!-- se non presente, poichè all'avvio della pagina web la prop films.length !> 0 segue che verrebbe eseguita questa condizione di v-else -->
            <!-- per un brevvissimo ma percettibile lasso di tempo, dopo il quale la prop films si popola è viene eseguito if -->
            {{delay()}}
            <div v-show="show">
                <span class="fs-1">Non ci sono film disponibili</span>
                <div>Hai cercato: "{{valoreCercato}}"</div>
            </div>
        </div>

    </section>
</template>

<script>

    import ShowCard from "./ShowCard.vue";

    export default {
        name: "FilmsList",
        components:{
            ShowCard,
        },
        data(){
            return{
                show: false,
                filmsCercati: true,
            }
        },
        props:{
            "films": Array,
            "valoreCercato": String
        },
        methods: {

            delay(){
                setTimeout(() => this.show = true ,200);
            },

            mostraNascondi(){
                this.filmsCercati = !this.filmsCercati;
            }

        }
        
    }
</script>

<style lang="scss" scoped>

    .categoria{
        cursor: pointer;

        &:hover{
            color: red;
        }
    }

</style>