<template>
  <div id="listContainer">
    <h2>{{ msg }}</h2>
    <template v-for="character in characters" :key="character.id">
      <div @click="characterClick(character.id)">
        {{ character.name }}
      </div>
    </template>
  </div>
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
    msg: "Character List",
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

<style lang="scss">
  #listContainer{
    width: 20%;
    margin-left: 40%;
    div {
      padding: 6px;
      cursor: pointer;
      font-weight: normal;
      &:hover {
        transition: 0.2s;
        background-color: #e8fffb;
        padding: 6px 12px;
        &::before{
          content: "\1F449 \00A0";
        }
      }
    }
  }
</style>
