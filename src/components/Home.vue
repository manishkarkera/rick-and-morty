<template>
  <h2>{{ msg }}</h2>
  <template v-for="character in characters" :key="character.id">
    <li @click="characterClick(character.id)">
      {{ character.name }}
    </li>
  </template>
  <character-details
    v-if="showCharacterDetails"
    @close="showCharacterDetails = false"
    :characterObj="characterObj"
  />
</template>

<script>
import CharacterDetails from "./CharacterDetails.vue";

export default {
  name: "Home",
  components: {
    CharacterDetails
  },
  props: {
    pageNo: {
      type: Number
    }
  },
  data: () => ({
    msg: "Lets goooo!!",
    characters: [],
    showCharacterDetails: false,
    characterIdInFocus: 0
  }),
  computed: {
    characterObj() {
      return this.characters.filter(
        ({ id }) => id == this.characterIdInFocus
      )[0];
    }
  },
  methods: {
    async load() {
      this.characters = await fetch(
        `https://rickandmortyapi.com/api/character/?page=${this.pageNo}`
      ).then(response => response.json())
        .then(({ results }) => this.characters = results)
    },
    characterClick(id) {
      this.characterIdInFocus = id;
      this.showCharacterDetails = true;
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
