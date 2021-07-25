<template>
  <header-bar v-model="searchTerm"/>
  <div id="listContainer">
    <template v-for="character in filteredCharacters" :key="character.id">
      <div @click="characterClick(character.id)">
        {{ character.name }}
      </div>
    </template>
  </div>
  <div id="footer">
    <button
      type="button"
      v-if="!!prevPageUrl"
      @click="load(prevPageUrl)"
    >
      Previous
    </button>
    <button
      type="button"
      v-if="!!nextPageUrl"
      @click="load(nextPageUrl)"
    >
      Next
    </button>
  </div>
  <character-details
    v-if="showCharacterDetails"
    @close="showCharacterDetails = false"
    :characterObj="characterObj"
  />
</template>

<script>
import HeaderBar from "./Header.vue";
import CharacterDetails from "./CharacterDetails.vue";

export default {
  name: "Home",
  components: {
    HeaderBar,
    CharacterDetails
  },
  data: () => ({
    apiResponse: {},
    showCharacterDetails: false,
    characterIdInFocus: 0,
    searchTerm: ""
  }),
  computed: {
    characterObj() {
      return this.apiResponse.results.filter(
        ({ id }) => id == this.characterIdInFocus
      )[0];
    },
    nextPageUrl() {
      return this.apiResponse &&
        this.apiResponse.info &&
        this.apiResponse.info.next
    },
    prevPageUrl(){
      return this.apiResponse &&
        this.apiResponse.info &&
        this.apiResponse.info.prev
    },
    filteredCharacters(){
      return this.apiResponse &&
        this.apiResponse.results &&
        this.apiResponse.results.filter(
          ({ name }) =>
            name.toLowerCase().includes(this.searchTerm.trim().toLowerCase())
        )
    }
  },
  methods: {
    async load(url) {
      this.characters = await fetch(url)
        .then(response => response.json())
        .then(response => this.apiResponse = response)
    },
    characterClick(id) {
      this.characterIdInFocus = id;
      this.showCharacterDetails = true;
    }
  },
  mounted() {
    this.load('https://rickandmortyapi.com/api/character/');
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
