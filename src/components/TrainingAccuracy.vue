<template>
  <div class="training__accuracy">Accuracy: {{ rerenderAccuracy }}% 🎯</div>
</template>

<script>
export default {
  name: "TrainingAccuracy",
  props: ['numberOfSymbols', 'isTyping', 'isTypo'],
  data() {
    return {
      accuracyValuePercent: 100,
      oneTypoPercent: 100 / this.numberOfSymbols,
    }
  },
  methods: {
    wasTypo() {
      if (this.isTyping && this.isTypo) {
        this.accuracyValuePercent -= this.oneTypoPercent;
      }
    }
  },
  computed: {
    rerenderAccuracy() {
      this.wasTypo();
      console.log('1');
      if (!Number.isInteger(this.accuracyValuePercent)) {
        return this.accuracyValuePercent.toFixed(1);
      }
      return this.accuracyValuePercent;
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
    font-weight: 600;
  }
</style>
