<template>
  <div id="overlay">
    <div id="characterCard">
      <div id="classHeader">
        <h3>{{ characterObj.name }}</h3>
        <div class="button" @click="$emit('close')">
          &#10060;
        </div>
      </div>
      <div class="contentRow1">
        <img :src="characterObj.image" width="300" />
        <div class="otherDetails">
          <p v-if="characterObj.type">
            <span>Type:</span>
            {{ characterObj.type }}
          </p>
          <p v-if="characterObj.status">
            <span>Status:</span>
            {{ characterObj.status }}
          </p>
          <p v-if="characterObj.origin.name">
            <span>Origin:</span>
            {{ characterObj.origin.name }}
          </p>
          <p v-if="characterObj.location.name">
            <span>Location:</span>
            {{ characterObj.location.name }}
          </p>
        </div>
      </div>
      <div class="contentRow2">
        <div
          class="episodeHeader"
          @click="showEpisodeList = !showEpisodeList"
        >
          Episode List
          <span>&#187;</span>
        </div>
        <transition name="episode">
        <div v-if="showEpisodeList" class="episodeBody">
          <ul>
            <template v-for="episode in characterEpisodes" :key="episode">
              <li>{{ episode }}</li>
            </template>
          </ul>
        </div>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CharacterDetails",
  props: {
    characterObj: {
      type: Object,
      required: true
    }
  },
  data: () => ({
    showEpisodeList: false
  }),
  computed: {
    characterEpisodes() {
      return this.characterObj.episode.map(q => `Episode ${q.split("/").pop()}`)
    }
  }
}
</script>

<style lang="scss">
  .contentRow1{
    padding: 20px 40px;
    display: flex;
  }
  #overlay{
    display: flex;
    align-items: center;
    justify-content: center;
    position: absolute;
    top: 0;
    left: 0;
    background-color: rgba(0,0,0,0.8);
    width: 100%;
    height: 100%;
  }
  .otherDetails{
    padding: 10px 20px;
    span:first-child{
      font-weight: bold
    }
  }
  #characterCard{
    background-color: #b7b7b7;
  }
  #classHeader{
    height: 60px;
    background-color: #2d312d;
    display: flex;
    justify-content: space-between;
    align-items: center;
    h3{
      color: white;
      padding: 0px 20px;
    }
    div.button {
      cursor: pointer;
      padding: 20px;
      &:hover {
        transition: 0.7s;
        background-color: #202420;
      }
    }
  }
  .contentRow2{
    text-align: center;
    .episodeHeader{
      font-weight: bold;
      padding: 15px;
      &:hover{
        transition: 0.7s;
        background-color: #6f6f6f;
        cursor: pointer
      }
    }
  }
  .episode-enter-active,
  .episode-leave-active {
    transition: height 1s ease;
  }

  .episode-enter-from,
  .episode-leave-to {
    height: 0;
  }

</style>
