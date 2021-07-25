<template>
  <input
    type="text"
    name="search"
    placeholder="Search Characters"
    v-model="modelValue"
    @keyup="$emit('update:modelValue', $event.target.value)"
  />
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
