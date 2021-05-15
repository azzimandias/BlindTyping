<template>
 <div class="training__text-body">
   <p class="training__text aquamarine">{{ usersText }}</p>
   <p class="training__text bisque" :class="{ tomato: isTypo }">{{ currentSymbol }}</p>
   <p class="training__text" v-if="!isLoaded">Receive text from the server...</p>
   <p class="training__text" v-else>{{ receivedText }}</p>
   <input
       id="super"
       type="text"
       v-model="inputSymbol"
       @keypress="symbolCompare($event)"
       @blur="onFocus()"
   />
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
        this.isLoaded = true;
        this.onFocus();
        this.symbolSubstitution();
      }).catch(err => {
        console.error(err);
        alert('Issue or error\n' + err);
      }),
      receivedText: '',
      usersText: '',
      currentSymbol: '',
      inputSymbol: '',
      isTypo: false,
      isLoaded: false,
    }
  },
  methods: {
    getApiText() {
      return fetch('https://baconipsum.com/api/?type=all-meat&sentences=' + this.sentencesNumber)
          .then(response => {
            return  response.json();
          });
    },
    symbolCompare(event) {
      if (event.key === this.currentSymbol || (event.key === ' ' && this.currentSymbol === ' ')) {
        this.usersText += event.key;
        this.inputSymbol = '';
        this.isTypo = false;
        this.symbolSubstitution();
      } else {
        this.isTypo = true;
        this.inputSymbol = '';
      }
    },
    symbolSubstitution() {
      if(!this.isTypo) {
        if (this.receivedText.charAt(0) === ' ') {
          this.currentSymbol = ' ';
        } else {
          this.currentSymbol = this.receivedText.charAt(0);
        }
        this.receivedText = this.receivedText.slice(1);
      }
    },
    onFocus() {
      let elem = document.getElementById("super");
      elem.focus();
    },
  }
}
</script>

<style scoped>
  .training__text-body {
    width: 100%;
    text-align: justify;
  }

  #super {
    position: absolute;
    top: -10%;
    left: 0;
  }

  .training__text {
    display: inline;
    font-size: 25px;
    font-weight: 400;
    line-height: 35px;
  }

  .aquamarine {
    color: aquamarine;
  }

  .bisque {
    display: inline-block;
    min-width: 18px;
    text-align: center;
    background-color: bisque;
    border: 1px solid bisque;
    border-radius: 5px;
  }

  .tomato {
    background-color: tomato;
  }
</style>
