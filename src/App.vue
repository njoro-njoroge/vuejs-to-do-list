<template>

 <Homepage />
</template>

<script>
import Homepage from './components/HomePage.vue';

export default {
  name: 'App',

  components:{
    Homepage
  }
 
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
