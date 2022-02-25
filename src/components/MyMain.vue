<template>
    <main class="container-fluid">
        <ul class="text-white mt-5">
            <div v-if="films.length > 0">
                <li class="mb-5 list-unstyled" v-for="(film, index) in films" :key="index">
                    <span>Film n° {{index+1}}:</span>
                    <ul>
                        <li>Titolo: {{film.title}} </li>
                        <li v-show="film.title!=film.original_title">Titolo Originale: {{film.original_title}} </li>
                        <li>Lingua: {{film.original_language}}</li>
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