<template>
  <div class="app">
    <nav class="controls">
      <div class="controls__body">
        <h2 class="controls__title">
          Blind Typing &#128064;
        </h2>
        <div class="controls__sentences">
          <h5 class="controls__sentences-title">
            Number of sentences:
          </h5>
          <select
              class="controls__sentences-select"
              v-model="sentencesNumber"
              v-bind:disabled="isTraining">
            <option>1</option>
            <option>5</option>
            <option>10</option>
            <option>15</option>
            <option>20</option>
          </select>
        </div>
        <StartButton
            @changeIsTraining="changeIsTraining()"
        />
      </div>
    </nav>

    <div class="training">
      <div class="training__body" :class="{ flex_sb: isTraining }">
        <div class="training__info-body" v-if="!isTraining">
          <h2 class="training__info-title">
            Blind typing training web application.<br/>
            Implemented using the Vue framework.<br/>
            Click 'Start training' to run. &#x1F468;&#x200D;&#x1F4BB;<br/>
          </h2>
        </div>

        <TrainingText
            :isTraining="isTraining"
            :sentencesNumber="sentencesNumber"
            v-else
        />
      </div>
    </div>

  </div>
</template>

<script>
import StartButton from "@/components/StartButton";
import TrainingText from "@/components/TrainingText";

export default {
  name: 'App',
  components: {
    StartButton,
    TrainingText,
  },
  data() {
    return {
      isTraining: false,
      sentencesNumber: 5,
    }
  },
  methods: {
    changeIsTraining() {
      this.isTraining = !this.isTraining;
    },
  },
  computed: {

  }
}
</script>

<style>
  * {
    //background-color: aliceblue;
    background-color: white;
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    border: none;
    -moz-user-select: none;
    user-select: none;
  }

  .app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    //color: dimgray;
  }

  .controls {
    width: 100%;
    margin: 10px 0;
    //border: 1px solid black;
  }

  .controls__body {
    max-width: 800px;
    height: 70px;
    margin: 0 auto;
    padding: 10px 30px;

    display: flex;
    justify-content: space-between;
    align-items: center;

    background-color: aliceblue;
    border: 1px solid black;
    border-radius: 5px;
  }

  .controls__title {
    min-width: 185px;
    margin-bottom: 5px;
    background-color: inherit;
  }

  .controls__sentences-select {
    background-color: aliceblue;
  }

  .controls__sentences-title {
    background-color: aliceblue;
  }

  .controls__sentences {
    min-width: 185px;
    display: flex;
    background-color: inherit;
  }

  .training__body {
    max-width: 800px;
    min-height: 700px;
    margin: 0 auto;
    padding: 10px 30px;

    display: flex;
    flex-direction: column;
    justify-content: center;

    background-color: aliceblue;
    border: 1px solid black;
    border-radius: 5px;
  }

  .flex_sb {
    justify-content: space-between;
  }

  .training__info-title {
    background-color: inherit;
  }

  .training__info-body  {
    position: relative;
    max-width: 75%;
    margin: 0 auto 30%;
    padding: 0 10px;
    background-color: inherit;
    //border: 1px solid black;
  }

  .training__info-body::after {
    content: '';
    position: absolute;
    left: calc(50% - 200px);
    bottom: -20%;
    width: 400px;
    height: 5px;
    background: #6D7278;
    border-radius: 5px;
  }
</style>
