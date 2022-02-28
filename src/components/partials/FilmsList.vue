<template>
    <section>

        <h1 v-show="films.length > 0">FILM</h1>
        <ul v-if="films.length > 0">

            <li class="mb-5 list-unstyled" 
                v-for="(film, index) in films" 
                :key="index">

            <ShowCard 
                :titolo="film.title" 
                :titolo_originale="film.original_title" 
                :lingua_originale="film.original_language" 
                :voto="film.vote_average"
                :poster_path="film.poster_path"/>
                
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

        }
        
        
    }
</script>

<style>

</style>