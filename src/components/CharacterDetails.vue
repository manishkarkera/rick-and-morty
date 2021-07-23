<template>
  <div id="characterPopup">
    <p> {{ characterObj.name }}</p>
    <button
      type="button"
      @click="$emit('close')"
    >
      Close
    </button>
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
  }
}
</script>

<style>
  #characterPopup{
    position: absolute;
    top: 0;
    background-color: red;
    width: 100%;
    height: 100%
  }
</style>
