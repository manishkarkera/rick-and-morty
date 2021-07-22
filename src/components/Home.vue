<template>
  <h2>{{ msg }}</h2>
  <template v-for="character in characters" :key="character.id">
    <li>{{ character.name }}</li>
  </template>
</template>

<script>
export default {
  name: "Home",
  data: () => ({
    pageNo: 1,
    msg: "Lets goooo!!",
    characters: []
  }),
  mounted() {
    fetch(`https://rickandmortyapi.com/api/character/?page=${this.pageNo}`)
      .then(response => response.json())
      .then(({ results }) => this.characters = results )
  }
}
</script>
