<template>
  <div id="characterPopup">
    <div id="characterCard">
      <img :src="characterObj.image" />
      <p>Name: {{ characterObj.name }}</p>
      <p>Status: {{ characterObj.status }}</p>
      <p>Type: {{ characterObj.type }}</p>
      <p>Origin: {{ characterObj.origin.name }}</p>
      <p>Location: {{ characterObj.location.name }}</p>
      <ul>
        <template v-for="episode in characterEpisodes" :key="episode">
          <li>{{ episode }}</li>
        </template>
      </ul>
      <button
        type="button"
        @click="$emit('close')"
      >
        Close
      </button>

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
  computed: {
    characterEpisodes() {
      return this.characterObj.episode.map(q => `Episode ${q.split("/").pop()}`)
    }
  }
}
</script>

<style>
  #characterPopup{
    position: absolute;
    top: 0;
    background-color: rgba(255,0,0,0.1);
    width: 100%;
    height: 100%;
  }
  #characterCard{
    background-color: white;
    width: 300px
  }
</style>
