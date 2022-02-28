<template>
    <main class="container-fluid">
        <div class="text-white mt-5">
            <div v-if="valoreCercato != null">
                <FilmsList :films="films" :valoreCercato="valoreCercato"/>
                <SeriesTvList :series="series" :valoreCercato="valoreCercato"/>
            </div>
            <div v-else class="text-center fs-1">
                <div class="m-5">Benvenuto in Boolflix</div>
                <div class="m-5">Adesso puoi guardare film e serie tv preferite stando comodamente seduto sul tuo divano</div>
                <div class="m-5">Cosa aspetti inizia subito una ricerca!!!</div>
            </div>
        </div>
    </main>
</template>

<script>

    import FilmsList from "./partials/FilmsList.vue";
    import SeriesTvList from "./partials/SeriesTvList.vue";

    export default {
        name: "MyMain",
        components:{
            FilmsList,
            SeriesTvList,
        },
        props:{
            "films": Array,
            "series": Array,
            "valoreCercato": String,
        },
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