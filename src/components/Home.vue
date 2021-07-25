<template>
  <div class="backgroundFilm">
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
  </div>
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
  .backgroundFilm {
    width: 100%;
    height: 100%;
    background-color: rgba(0,0,0,0.8);
    position: absolute;
    left: 0;
  }
  body{
    background-image: url("../../public/r&m.jpg");
    height: 100vh;
  }
  #listContainer{
    background: #1c1c1c;
    color: #dbdbdb;
    margin-top: 15vh;
    width: 20%;
    margin-left: 40%;
    div {
      font-weight: bold;
      padding: 6px;
      cursor: pointer;
      font-weight: normal;
      &:hover {
        transition: 0.2s;
        background-color: #111;
        padding: 6px 12px;
        &::before{
          content: "\1F449 \00A0";
        }
      }
    }
  }
</style>
