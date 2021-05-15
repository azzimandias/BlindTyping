<template>
 <div class="training__text-body">
   <p class="training__text">{{ receivedText }}</p>
 </div>
</template>

<script>
export default {
  name: "TrainingText",
  props: ['isTraining', 'sentencesNumber'],
  data() {
    return {
      initText: this.getApiText().then(data => {
        this.receivedText = data[0];
      }).catch(err => {
        console.error(err);
        alert('Issue or error\n' + err);
      }),
      receivedText: 'Receive text from the server...',
    }
  },
  methods: {
    getApiText() {
      return fetch('https://baconipsum.com/api/?type=all-meat&sentences=' + this.sentencesNumber)
          .then(response => {
            return  response.json();
          });
    }
  }
}
</script>

<style scoped>
  .training__text-body {
    width: 100%;
    //height: 25%;
    text-align: justify;
    //border: 1px solid black;
  }

  .training__text {
    font-size: 25px;
    font-weight: 400;
    -moz-user-select: none;
    user-select: none;
  }
</style>
