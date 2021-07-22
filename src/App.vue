<template>
  <Home :pageNo="pageNo" />
  <Paginator v-model="pageNo" />
</template>

<script>
import Home from './components/Home.vue';
import Paginator from './components/Paginator.vue';

export default {
  data: () => ({
    pageNo: 1
  }),
  components: {
    Home,
    Paginator
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
