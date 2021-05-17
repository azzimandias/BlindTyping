<template>
  <div class="SB">
    <button
        class="SB__body-1"
        v-if="!isClicked"
        @mouseup="changeIsClicked()"
    >▶ Start training</button>
    <button
        class="SB__body-2"
        v-else
        @mouseup="changeIsClicked()"
    >◼ Stop training</button>
  </div>
</template>

<script>
export default {
  name: "StartButton",
  data() {
    return {
      isClicked: false,
    }
  },
  methods: {
    changeIsClicked() {
      this.isClicked = !this.isClicked;
      this.$emit('changeIsTraining');
    }
  }
}
</script>

<style scoped>
  .SB {
    background-color: aliceblue;
  }

  .SB__body-1 {
    min-width: 185px;
    height: 40px;
    background-color: aquamarine;
    font-size: 15px;
    font-weight: 600;
    border: 1px solid black;
    border-radius: 20px;
  }

  .SB__body-2 {
    min-width: 185px;
    height: 40px;
    background-color: tomato;
    font-size: 15px;
    font-weight: 600;
    border: 1px solid black;
    border-radius: 20px;
  }
</style>
