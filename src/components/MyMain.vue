<template>
    <main id="container">
        <ul>
            <li class="mb-5 list-unstyled" v-for="(film, index) in films" :key="index">
                <span>Film n° {{index+1}}:</span>
                <ul>
                    <li>Titolo: {{film.title}} </li>
                    <li>Titolo Originale: {{film.original_title}} </li>
                    <li>Lingua: {{film.original_language}}</li>
                    <li>Voto: {{film.vote_average}}</li>
                </ul>
            </li>
        </ul>
    </main>
</template>

<script>
    export default {
        name: "MyMain",
        props:{
            "films": Array,
        },
    }
</script>

<style scoped>
    #container{
        height: calc(100vh - 4rem);
        background-color: grey;

        overflow: scroll;
    }
</style>