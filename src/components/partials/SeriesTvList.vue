<template>
    <section>

        <h1 class="categoria d-inline" v-show="series.length > 0"  @click="mostraNascondi()">SERIE TV</h1>

        <ul v-if="series.length > 0" class="row" v-show="serieCercate">

            <li class="col mb-5 list-unstyled" 
                v-for="(serie, index) in series" 
                :key="index">

            <ShowCard 
                :titolo="serie.name" 
                :titolo_originale="serie.original_name" 
                :lingua_originale="serie.original_language" 
                :voto="serie.vote_average"
                :poster_path="serie.poster_path"
                :overview="serie.overview"
                :genres="serie.generi"
                :cast="serie.cast"/>

            </li>

        </ul>
        <div v-else>
            <!-- delay() serve per gestire il primissimo caso, dopo aver lanciato la pagina web -->
            <!-- se non presente, poichè all'avvio della pagina web la prop series.length !> 0 segue che verrebbe eseguita questa condizione di v-else -->
            <!-- per un brevvissimo ma percettibile lasso di tempo, dopo il quale la prop series si popola è viene eseguito if -->
            {{delay()}}
            <div v-show="show">
                <span class="fs-1">Non ci sono serie tv disponibili</span>
                <div>Hai cercato: "{{valoreCercato}}"</div>
            </div>
        </div>

    </section>
</template>

<script>

    import ShowCard from "./ShowCard.vue";

    export default {
        name: "SeriesTvList",
        components:{
            ShowCard,
        },
        data(){
            return{
                show: false,
                serieCercate: true,
            }
        },
        props:{
            "series": Array,
            "valoreCercato": String
        },
        methods: {

            delay(){
                setTimeout(() => this.show = true ,200);
            },

            mostraNascondi(){
                this.serieCercate = !this.serieCercate;
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