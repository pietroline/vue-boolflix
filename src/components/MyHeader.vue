<template>
    <header class="container-fluid">
        <div class="row row-cols-1 row-cols-md-2 align-items-center">
            <div class="col d-flex justify-content-center justify-content-md-start">
                <MainLogo/>
            </div>
            <div class="col">
                <form class="m-2 d-flex justify-content-center justify-content-md-end">
                    <input class="form-control ms_width_20rem" placeholder="Cerca un film o serie tv...." type="text" v-model="ricerca">
                    <button type="submit" class="btn btn-light ms-2" @click.prevent="avviaRicerca()">Ricerca</button>
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
        },
        methods:{
            avviaRicerca(){
                if(this.ricerca != ""){
                    this.$emit('ricerca', this.ricerca);
                    setTimeout(()=>{
                        this.ricerca=""
                    },100);
                }
            }
        }
    }
</script>

<style lang="scss" scoped>

    @import "./../assets/variables.scss";

    .container-fluid{
        background-color: $colorHeader;
        
        line-height: 4rem;

        .ms_width_20rem{
            width: 20rem;
        }
    }

</style>