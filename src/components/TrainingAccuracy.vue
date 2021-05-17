<template>
  <div class="training__accuracy">Accuracy: {{ rerenderAccuracy }}% 🎯</div>
</template>

<script>
export default {
  name: "TrainingAccuracy",
  props: ['numberOfSymbols', 'isTypo'],
  data() {
    return {
      accuracyValuePercent: 100,
      oneTypoPercent: (100/this.numberOfSymbols).toFixed(1),
    }
  },
  methods: {
    wasTypo() {
      if (this.isTypo) {
        console.log('this.oneTypoPercentP')
        this.accuracyValuePercent -= this.oneTypoPercent;
      }
    }
  },
  computed: {
    rerenderAccuracy() {
      this.wasTypo();
      return this.accuracyValuePercent.toFixed(1);
    }
  }
}
</script>

<style scoped>
  .training__accuracy {
    flex-grow: 1;
    min-height: 20px;
    text-align: center;
    margin: auto 0;
    background-color: transparent;
    //border: 1px solid black;
  }
</style>
