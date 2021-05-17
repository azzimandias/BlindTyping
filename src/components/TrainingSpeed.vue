<template>
  <div class="training__speed">Speed: {{ rerenderSpeed }}cpm 🏎️</div>
</template>

<script>
export default {
  name: "TrainingSpeed",
  props: ['usersText', 'isTyping'],
  data() {
    return {
      typingSpeed: 0,
      totalTypingSpeed: 0,
      usersTextLengthOld: this.usersText.length,
      totalLengthSpeed: 0,
    }
  },
  methods: {
    wasTyping() {
      return setInterval(this.interval, 1000);
    },
    interval() {
      if (this.usersText.length - this.usersTextLengthOld) {
        if (this.isTyping) {
          this.typingSpeed += (this.usersText.length - this.usersTextLengthOld) * 60;
          this.totalLengthSpeed++;
          this.usersTextLengthOld = this.usersText.length;
          this.totalTypingSpeed = Math.ceil(this.typingSpeed / this.totalLengthSpeed);
        } else {

         //
        }
      }
    }
  },
  computed: {
    rerenderSpeed() {
      this.wasTyping();
      return this.totalTypingSpeed;
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
    background-color: inherit;
    //border: 1px solid black;
  }
</style>
