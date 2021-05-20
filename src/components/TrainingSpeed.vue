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
      minusSpeed: 0,
      minute: 0,
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
            this.typingSpeed -= this.minusSpeed;
            this.minusSpeed = 0;
            this.totalLengthSpeed++;
            this.typingSpeed += this.symbolsPerMinuteInSec;
            this.totalTypingSpeed = this.typingSpeed / this.totalLengthSpeed;
            this.usersTextLengthOld = this.usersText.length;
            this.minute = 60;
          } else {
            if (!this.minute) return;
            this.totalTypingSpeed -= this.symbolsPerSecond;
            this.minusSpeed += this.symbolsPerSecond;
            this.minute--;
          }
          /*console.log(this.usersText.length);
          console.log(Math.round(this.totalTypingSpeed));*/
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
      //return parseFloat(((this.typingSpeed / this.totalLengthSpeed) / 60).toFixed(2));
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

  @media screen and (max-width: 696px) {
    .training__speed {
      font-size: 13px;
    }
  }

  @media screen and (max-width: 375px) {
    .training__speed {
      font-size: 10px;
    }
  }
</style>
