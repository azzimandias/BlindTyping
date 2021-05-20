<template>
  <div class="SB">
    <button
        class="SB__body"
        :class="{ SB__body_1: isButtonClicked, SB__body_2: trainingIsEnded }"
        @mouseup="changeIsButtonClicked()"
    >{{ buttonStr }}</button>
  </div>
</template>

<script>
export default {
  name: "StartButton",
  props: ['trainingIsEnded'],
  data() {
    return {
      isButtonClicked: false,
      strStart: '▶ Start training',
      strStop: '◼ Stop training',
      strMenu: 'To main menu',
    }
  },
  methods: {
    changeIsButtonClicked() {
      if (this.isButtonClicked) this.$emit('changeTrainingIsEnded');
      this.isButtonClicked = !this.isButtonClicked;
      this.$emit('changeIsTraining');
    }
  },
  computed: {
    buttonStr() {
      if (this.trainingIsEnded) {
        return this.strMenu;
      } else if (this.isButtonClicked) {
        return this.strStop;
      } else {
        return this.strStart;
      }
    }
  }
}
</script>

<style scoped>
  .SB__body {
    min-width: 185px;
    height: 40px;
    background-color: darkseagreen;
    font-size: 15px;
    font-weight: 600;
    border: 1px solid black;
    border-radius: 20px;
  }

  .SB__body_1 {
    background-color: crimson;
  }

  .SB__body_2 {
    background-color: cadetblue;
  }

  @media screen and (max-width: 703px) {
    .SB__body {
      min-width: 155px;
    }
  }

  @media screen and (max-width: 549px) {
    .SB__body {
      min-width: 100px;
      font-size: 12px;
    }
  }

  @media screen and (max-width: 425px) {
    .SB__body {
      min-width: 90px;
      font-size: 8px;
    }
  }
</style>
