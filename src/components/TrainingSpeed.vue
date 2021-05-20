<template>
  <div class="training__speed" >Speed: {{ wasTyping() }}cpm 🐌️</div>
</template>

<script>
export default {
  name: "TrainingSpeed",
  props: ['usersText', 'isTyping', 'trainingIsEnded'],
  data() {
    return {
      typingSpeed: 0,
      totalTypingSpeed: 0,
      usersTextLengthOld: this.usersText.length,
      totalLengthSpeed: 0,
      minute: 60,
      flag: true,
      timer: '',
    }
  },
  methods: {
    wasTyping() {
      if (this.flag && this.isTyping) {
        this.flag = false;
        this.timer = setInterval(() => {
          if (this.trainingIsEnded) {
            clearInterval(this.timer);
          } else if (this.usersText.length - this.usersTextLengthOld) {
            this.totalLengthSpeed++;
            this.typingSpeed += this.symbolsPerMinuteInSec;
            this.totalTypingSpeed = this.typingSpeed / this.totalLengthSpeed;
            this.usersTextLengthOld = this.usersText.length;
            this.minute = 60;
          } else {
            if (!this.minute) return;
            this.totalTypingSpeed -= this.symbolsPerSecond;
            this.typingSpeed -= this.symbolsPerSecond;
            this.minute--;
          }
        }, 1000);
      }
      return Math.round(this.totalTypingSpeed);
    },
  },
  computed: {
    symbolsPerMinuteInSec() {
      return (this.usersText.length - this.usersTextLengthOld) * 60;
    },
    symbolsPerSecond() {
      return (this.typingSpeed / this.totalLengthSpeed) / 60;
    }
  }
}
</script>

<style scoped>
  .training__speed {
    flex-grow: 1;
    min-height: 20px;
    text-align: center;
    margin: auto 0;
    font-weight: 600;
  }
</style>
