<template>
  <home :pageNo="pageNo" />
  <paginator v-model="pageNo" />
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
