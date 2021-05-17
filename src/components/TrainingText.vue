<template>
 <div class="training__text-body">
   <div class="training__typing">
     <p class="training__text aquamarine">{{ usersText }}</p>
     <p class="training__text bisque" :class="{ tomato: isTypo }">{{ currentSymbol }}</p>
     <p class="training__text" v-if="!isLoaded">Receive text from the server...</p>
     <p class="training__text" v-else>{{ receivedText }}</p>
     <input
         id="super"
         type="text"
         v-model="inputSymbol"
         @keypress="eva($event)"
         @blur="onFocus()"
     />
   </div>
   <div class="training__watchers">
     <TrainingAccuracy
         :numberOfSymbols="numberOfSymbols"
         :isTypo="isTypo"
         :key="rerender"
     />
     <TrainingSpeed
        :usersText="usersText"
        :isTyping="isTyping"
     />
   </div>
 </div>
</template>

<script>
import TrainingAccuracy from "@/components/TrainingAccuracy";
import TrainingSpeed from "@/components/TrainingSpeed";

export default {
  name: "TrainingText",
  components: {
    TrainingAccuracy,
    TrainingSpeed,
  },
  props: ['isTraining', 'sentencesNumber'],
  data() {
    return {
      initText: this.getApiText().then(data => {
        this.receivedText = data[0];
        this.numberOfSymbols = this.receivedText.length;
        this.forceRerender();
        this.isLoaded = true;
        this.onFocus();
        this.symbolSubstitution();
      }).catch(err => {
        console.error(err);
        alert('Issue or error\n' + err);
      }),
      rerender: 0,
      numberOfSymbols: 0,
      receivedText: '',
      usersText: '',
      currentSymbol: '',
      inputSymbol: '',
      isTypo: false,
      isTyping: false,
      isLoaded: false,
    }
  },
  methods: {
    getApiText() {
      return fetch('https://baconipsum.com/api/?type=all-meat&sentences=' +
                    this.sentencesNumber)
          .then(response => {
            return  response.json();
          });
    },
    eva(event) {
      this.symbolCompare(event);
      this.isTyping = true;
    },
    symbolCompare(event) {
      if (event.key === this.currentSymbol ||
          (event.key === ' ' && this.currentSymbol === '\u00A0')) {
        this.usersText += event.key;
        this.inputSymbol = '';
        this.isTypo = false;
        this.symbolSubstitution();
        this.isTyping = false;
      } else {
        this.isTypo = true;
        this.inputSymbol = '';
        TrainingAccuracy.methods.wasTypo();
      }
    },
    symbolSubstitution() {
      if(!this.isTypo) {
        if (this.receivedText.charAt(0) === ' ') {
          this.currentSymbol = '\u00A0';
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
    forceRerender() {
      this.rerender++;
    }
  }
}
</script>

<style scoped>
  .training__text-body {
    width: 100%;
    height: 100%;
    text-align: justify;
    flex-grow: 1;

    display: flex;
    flex-direction: column;
    justify-content: space-between;

    background-color: transparent;
    //border: 1px solid white;
  }

  #super {
    position: absolute;
    top: -10%;
    left: 0;
  }

  .training__typing {
    background-color: transparent;
  }

  .training__text {
    display: inline;
    font-size: 25px;
    font-weight: 400;
    line-height: 35px;
    background-color: transparent;
  }

  .training__watchers {
    display: flex;
    justify-content: space-around;
    background-color: inherit;
  }

  .aquamarine {
    color: aquamarine;
  }

  .bisque {
    display: inline-block;
    text-align: center;
    background-color: bisque;
    border: 1px solid bisque;
    border-radius: 5px;
  }

  .tomato {
    background-color: tomato;
    border: 1px solid tomato;
  }
</style>
