<template>
  <h2>{{ msg }}</h2>
  <template v-for="character in characters" :key="character.id">
    <li>{{ character.name }}</li>
  </template>
</template>

<script>
export default {
  name: "Home",
  props: {
    pageNo: {
      type: Number
    }
  },
  data: () => ({
    msg: "Lets goooo!!",
    characters: [],
  }),
  methods: {
    async load() {
      this.characters = await fetch(
        `https://rickandmortyapi.com/api/character/?page=${this.pageNo}`
      ).then(response => response.json())
        .then(({ results }) => this.characters = results)
    }
  },
  mounted() {
    this.load();
  },
  watch: {
    pageNo() {
      this.load();
    }
  }
}
</script>
