<template>
  <div id="app">
    
    <MyHeader/>

    <MyMain/>
  </div>
</template>

<script>

  import MyHeader from "./components/MyHeader";
  import MyMain from "./components/MyMain";

  export default {
    name: 'App',
    components: {
      MyHeader,
      MyMain

    }
  }
</script>

<style lang="scss">

</style>
