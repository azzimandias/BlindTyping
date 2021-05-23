<template>
 <div class="training__text-body">
   <div class="training__typing" v-show="!trainingIsEnded">
     <p class="training__text aquamarine">{{ usersText }}</p>
     <p class="training__text bisque" :class="{
       tomato: this.isTypo,
       space: this.isInlineBlock
     }"
     >{{ currentSymbol }}</p>
     <p class="training__text" v-if="!isLoaded">Receive text from the server...</p>
     <p class="training__text" v-else>{{ receivedText }}</p>
     <input
         id="super"
         type="text"
         name="text"
         v-model="inputSymbol"
         @keypress="eventTransfer($event)"
         @blur="onFocus()"
     />
   </div>
   <div class="training__watchers" :class="{ training_end: this.trainingIsEnded}">
     <h2 class="training__watchers-title" v-if="trainingIsEnded">Congratulations!<br/>Your results:</h2>
     <div class="training__watchers-results"
          :class="{ 'training__watchers-results_half': trainingIsEnded}">
       <TrainingAccuracy
           :numberOfSymbols="numberOfSymbols"
           :isTyping="isTyping"
           :isTypo="isTypo"
           :key="keyToRerender"
       />
       <TrainingSpeed
          :usersText="usersText"
          :isTyping="isTyping"
          :trainingIsEnded="trainingIsEnded"
       />
     </div>
     <StartOverButton
         v-if="trainingIsEnded"
         @rerenderTrainingText="rerenderTrainingText()"
     />
   </div>
 </div>
</template>

<script>
import TrainingAccuracy from "@/components/TrainingAccuracy";
import TrainingSpeed from "@/components/TrainingSpeed";
import StartOverButton from "@/components/StartOverButton";

export default {
  name: "TrainingText",
  components: {
    TrainingAccuracy,
    TrainingSpeed,
    StartOverButton,
  },
  props: ['isTraining', 'trainingIsEnded', 'sentencesNumber'],
  data() {
    return {
      initText: this.getApiText().then(data => {
        this.receivedText = data[0];
        this.numberOfSymbols = this.receivedText.length;
        this.forceRerenderComponent();
        this.isLoaded = true;
        this.onFocus();
        this.symbolSubstitution();
      }).catch(err => {
        console.error(err);
        alert('Issue or error\n' + err);
      }),
      keyToRerender: 0,
      numberOfSymbols: 0,
      receivedText: '',
      usersText: '',
      currentSymbol: '',
      inputSymbol: '',
      isTypo: false,
      isTyping: false,
      isLoaded: false,
      isInlineBlock: false,
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
    eventTransfer(event) {
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
      }
    },
    symbolSubstitution() {
      if (this.receivedText.charAt(0) === ' ') {
        this.currentSymbol = '\u00A0';
        this.isInlineBlock = true;
      } else if (!this.receivedText) {
        this.$emit('changeTrainingIsEnded');
      } else {
        this.currentSymbol = this.receivedText.charAt(0);
        this.isInlineBlock = false;
      }
      this.receivedText = this.receivedText.slice(1);
    },
    onFocus() {
      let elem = document.getElementById("super");
      if (elem) {
        elem.focus();
        elem.click();
      }
    },
    forceRerenderComponent() {
      this.keyToRerender++;
    },
    rerenderTrainingText() {
      this.$emit('rerenderTrainingText');
    }
  }
}
</script>

<style scoped>
  .training__text-body {
    width: 100%;
    min-height: calc(100vh - 122px);
    text-align: left;

    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }

  #super {
    position: absolute;
    top: -10%;
    left: 0;
    opacity: 0;
  }

  .training__text {
    display: inline;
    font-size: 25px;
    font-weight: 400;
    line-height: 35px;
  }

  .training__watchers {
    display: flex;
    margin-top: 30px;
  }

  .training_end {
    flex-direction: column;
    align-items: center;
    margin-top: 100px;
    margin-bottom: auto;
  }

  .training__watchers-title {
    text-align: center;
  }

  .training__watchers-results {
    display: flex;
    width: 100%;
    margin: 50px 0;
    justify-content: space-between;
  }

  .training__watchers-results_half {
    width: 50%;
  }

  .aquamarine {
    color: darkseagreen;
  }

  .bisque {
    text-align: center;
    background-color: bisque;
    border-radius: 5px;
  }

  .space {
    display: inline-block;
  }

  .tomato {
    background-color: crimson;
  }

  @media screen and (max-width: 768px) {
    .training__text {
      font-size: 20px;
    }
  }

  @media screen and (max-width: 549px) {
    .training__text {
      font-size: 16px;
    }
  }

  @media screen and (max-width: 425px) {
    .training__text {
      font-size: 13px;
    }
  }

  @media screen and (max-width: 375px) {
    .training__text {
      font-size: 10px;
    }
  }

  @media screen and (max-width: 320px) {
    .training__text {
      font-size: 8px;
    }
  }
</style>
