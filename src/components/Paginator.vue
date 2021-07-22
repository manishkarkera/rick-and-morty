<template>
  <button type="button" name="button" @click="actionPrevPage">Previous</button>
  <button type="button" name="button" @click="actionNextPage">Next</button>
</template>

<script>
export default {
  name: "Paginator",
  props: {
    modelValue: {
      type: Number
    }
  },
  methods: {
    actionNextPage() {
      this.$emit("update:modelValue", this.modelValue + 1)
    },
    actionPrevPage() {
      const updatedPageNo = this.modelValue - 1;
      !!updatedPageNo && this.$emit("update:modelValue", updatedPageNo);
    }
  }
}
</script>
