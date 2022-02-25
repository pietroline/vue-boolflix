<template>
    <header id="container">
        <div class="row row-cols-1">
            <div class="col d-flex align-items-center justify-content-between px-4">
                <MainLogo/>
                <form class="d-flex">
                    <input class="form-control" type="text" v-model="ricerca">
                    <button type="submit" class="btn btn-light ms-2" @click.prevent="$emit('ricerca', ricerca)">Ricerca</button>
                </form>
            </div>           
        </div>
    </header>
</template>

<script>

    import MainLogo from "./partials/MainLogo.vue";

    export default {
        name:"MyHeader.vue",
        components:{
            MainLogo,
        },
        data(){
            return{
                ricerca: "",
            }
        }
    }
</script>

<style lang="scss" scoped>

    #container{
        background-color: #000;
        height: 4rem;
    }

</style>