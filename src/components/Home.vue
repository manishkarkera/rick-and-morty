<template>
  <div class="backgroundFilm">
    <header-bar v-model="searchTerm"/>
    <div id="listContainer">
      <template v-if="!!filteredCharacters && !filteredCharacters.length">
        <p>No character named &quot;<strong>{{ searchTerm }}</strong>&quot;</p>
      </template>
      <template v-for="character in filteredCharacters" :key="character.id">
        <div @click="characterClick(character.id)">
          {{ character.name }}
        </div>
      </template>
    </div>
    <div id="footer">
      <div class="prevPage"
        v-if="!!prevPageUrl"
        @click="load(prevPageUrl)"
      >
        Prev page
        <span>&#171;&nbsp;</span>
      </div>
      <div class="pagePosition">
        {{ pagePosition }}
      </div>
      <div class="nextPage"
        v-if="!!nextPageUrl"
        @click="load(nextPageUrl)"
      >
      <span>&#187;&nbsp;</span>
        Next page
      </div>
    </div>
    <transition name="popup">
      <character-details
      v-if="showCharacterDetails"
      @close="showCharacterDetails = false"
      :characterObj="characterObj"
      />
    </transition>
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
    },
    pagePosition() {
      return this.apiResponse &&
        this.apiResponse.info &&
        `${parseInt(this.apiResponse.info.next.split("=")[1]) - 1} of ${this.apiResponse.info.pages}`;
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
.popup-enter-active,
.popup-leave-active {
  transition: opacity 0.1s ease;
}

.popup-enter-from,
.popup-leave-to {
  opacity: 0;
}
  .backgroundFilm {
    width: 100%;
    height: 100%;
    background-color: rgba(0,0,0,0.7);
    position: absolute;
    left: 0;
  }
  body{
    background-image: url("r&m.jpg");
    height: 100vh;
  }
  #listContainer{
    padding: 10px 30px;
    background: #1c1c1c;
    color: #dbdbdb;
    margin-top: 15vh;
    width: 20%;
    margin-left: 40%;
    border: 3px #6f6f8e solid;
    border-radius: 9px;
    box-shadow: 5px 5px #39393980;
    div {
      font-weight: bold;
      padding: 6px;
      cursor: pointer;
      font-weight: normal;
      &:hover {
        transition: 0.2s;
        background-color: #111;
        padding: 6px 12px;
        border-radius: 5px;
        &::before{
          content: "\1F449 \00A0";
        }
      }
    }
  }
  #footer{
    color: white;
    display: flex;
    justify-content: center;
    background-color: #0d0d0d;
    position: fixed;
    bottom: 0;
    width: 100%;
    height: 7vh;
    align-items: center;
    .pagePosition{
      padding: 10px;
    }
    .prevPage,
    .nextPage{
      height: 100%;
      padding: 0px 15px;
      display: flex;
      align-items: center;
      span{
        font-size: 30px;
      }
      &:hover{
        background-color: black;
        cursor: pointer;
        transition: 0.3s;
      }
    }
  }
</style>
