<template>
  <div class="container">
    <img src="favicon.ico" alt="Rick's face" style="height: 100%">
    <h2>Characters database</h2>
    <div class="searchBar">
      <input
        type="text"
        name="search"
        placeholder="Search Characters"
        v-model="modelValue"
        @keyup="$emit('update:modelValue', $event.target.value)"
      />
      <span>&#128269;</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "Header",
  props: {
    modelValue: {
      type: String
    }
  }
}
</script>

<style scoped lang="scss">
  .container{
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 10vh;
    display: flex;
    align-items: center;
    background-color: #2f2e2e;
    border-bottom: #6f6f8e solid 5px;
    margin-bottom: 70px;
    img {
      padding: 10px 20px;
    }
    h2{
      color: white;
    }
    .searchBar{
      position: absolute;
      right: 10%;
      span{
        padding: 10px;
      }
    }
  }
</style>
