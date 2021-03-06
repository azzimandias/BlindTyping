<template>
  <div class="app">
    <nav class="controls">
      <div class="controls__body">
        <h2 class="controls__title">
          Blind Typing &#128064;
        </h2>
        <ThemeSwitcher/>
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
          </select>
        </div>
        <StartButton
            :trainingIsEnded="trainingIsEnded"
            @changeIsTraining="changeIsTraining()"
            @changeTrainingIsEnded="changeTrainingIsEnded()"
        />
      </div>
    </nav>
    <div class="training">
      <div class="training__body">
        <div class="training__info-body" v-if="!isTraining">
          <h2 class="training__info-title">
            Blind typing training web application.<br/>
            Implemented using the Vue framework.<br/>
            Click 'Start training' to run. &#x1F468;&#x200D;&#x1F4BB;<br/>
          </h2>
        </div>
        <TrainingText
            :isTraining="isTraining"
            :trainingIsEnded="trainingIsEnded"
            :sentencesNumber="sentencesNumber"
            :key="keyToRerender"
            @changeTrainingIsEnded="changeTrainingIsEnded()"
            @rerenderTrainingText="rerenderTrainingText()"
            v-else
        />
      </div>
    </div>
  </div>
</template>

<script>
import ThemeSwitcher from "./components/ThemeSwitcher";
import StartButton from "@/components/StartButton";
import TrainingText from "@/components/TrainingText";

export default {
  name: 'App',
  components: {
    ThemeSwitcher,
    StartButton,
    TrainingText,
  },
  data() {
    return {
      isTraining: false,
      trainingIsEnded: false,
      sentencesNumber: 1,
      keyToRerender: 0,
    }
  },
  methods: {
    changeIsTraining() {
      this.isTraining = !this.isTraining;
      if (this.trainingIsEnded) this.changeTrainingIsEnded();
    },
    changeTrainingIsEnded() {
      this.trainingIsEnded = !this.trainingIsEnded;
    },
    rerenderTrainingText() {
      this.changeTrainingIsEnded();
      this.keyToRerender++;
    }
  }
}
</script>

<style>
  * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    border: none;
    -moz-user-select: none;
    user-select: none;
  }

  body {
    background-color: var(--back-color);
    transition: 0.3s;
  }

  div, select, h2, h5, p {
    background-color: inherit;
  }

  button, select {
    color: var(--text-color);
  }

  .app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: var(--text-color);
  }

  .controls {
    width: 100%;
    margin: 10px 0;
    padding: 0 10px;
  }

  .controls__body {
    max-width: 800px;
    height: 70px;
    margin: 0 auto;
    padding: 10px 30px;

    display: flex;
    justify-content: space-between;
    align-items: center;

    background-color: var(--bodys-back);
    border: 1px solid var(--borders-color);
    border-radius: 5px;
  }

  .controls__title {
    min-width: 185px;
    min-height: 48px;
    padding-top: 6px;
    text-align: center;
  }

  .controls__sentences {
    min-width: 185px;
    min-height: 48px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .training {
    padding: 0 10px;
  }

  .training__body {
    max-width: 800px;
    min-height: calc(100vh - 100px);
    margin: 0 auto 10px;
    padding: 10px 30px;

    background-color: var(--bodys-back);
    border: 1px solid var(--borders-color);
    border-radius: 5px;
  }

  .training__info-body  {
    position: relative;
    max-width: 75%;
    margin: 100px auto auto;
    padding: 0 10px;
  }

  .training__info-title {
    line-height: 50px;
  }

  .training__info-body::after {
    content: '';
    position: absolute;
    left: calc(50% - 200px);
    bottom: -20%;
    width: 400px;
    height: 5px;
    background: var(--training-back);
    border-radius: 5px;
  }

  @media screen and (max-width: 780px) {
    .controls__title {
      min-width: 155px;
      font-size: 18px;
      padding-top: 10px;
    }

    .controls__sentences {
      min-width: 155px;
      flex-direction: column;
      min-height: auto;
    }

    .controls__sentences-title {
      font-size: 10px;
    }

    .training__info-title {
      font-size: 20px;
    }

    @media screen and (max-width: 549px) {
      .controls__body {
        padding: 10px 15px;
      }

      .controls__title {
        min-width: 100px;
        font-size: 15px;
        padding-top: 12px;
      }

      .controls__sentences {
        min-width: 100px;
      }

      .controls__sentences-title {
        font-size: 10px;
      }

      .training__info-title {
        font-size: 13px;
      }

      .training__info-body::after {
        left: calc(50% - 100px);
        width: 200px;
      }
    }

    @media screen and (max-width: 425px) {
      .controls__title {
        min-width: 90px;
        font-size: 11px;
        padding-top: 16px;
      }

      .controls__sentences {
        min-width: 90px;
      }

      .controls__sentences-title {
        display: none;
      }

      .training__info-title {
        font-size: 10px;
      }
    }

    @media screen and (max-width: 425px) {
      .training__info-title {
        font-size: 8px;
      }

      .training__info-body::after {
        height: 2px;
      }
    }
  }
</style>
