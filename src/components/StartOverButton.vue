<template>
  <div class="SOB">
    <button
        class="SOB__body"
        @mouseup="isButtonClicked()"
    >↩ Start over</button>
  </div>
</template>

<script>
export default {
  name: "StartOverButton",
  methods: {
    isButtonClicked() {
      this.$emit('rerenderTrainingText');
    }
  }
}
</script>

<style scoped>
  .SOB__body {
    width: 185px;
    height: 40px;
    background-color: bisque;
    font-size: 15px;
    font-weight: 600;
    border: 1px solid black;
    border-radius: 20px;
  }
</style>
