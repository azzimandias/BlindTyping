<template>
  <div>
    <button
        class="SB__body-1"
        v-if="!isClicked"
        @mouseup="changeIsTraining()"
    >▶ Start training
    </button>
    <button
        class="SB__body-2"
        v-else
        @mouseup="changeIsTraining()"
    >◼ Stop
    </button>
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
    changeIsTraining() {
      this.isClicked = !this.isClicked;
      this.$emit('changeIsTraining');
    }
  }
}
</script>

<style scoped>
  .SB__body-1 {
    width: 150px;
    height: 40px;
    background-color: aquamarine;
    font-size: 15px;
    font-weight: 600;
    border: 1px solid black;
    border-radius: 20px;
  }

  .SB__body-2 {
    width: 150px;
    height: 40px;
    background-color: tomato;
    font-size: 15px;
    font-weight: 600;
    border: 1px solid black;
    border-radius: 20px;
  }
</style>
