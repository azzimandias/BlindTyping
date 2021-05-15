<template>
  <div class="app">
    <nav class="controls">
      <div class="controls__body">
        <h2 class="controls__title">
          Blind Typing
        </h2>
        <StartButton
            @changeIsTraining="changeIsTraining()"
        />
      </div>
    </nav>

    <div class="training">
      <div class="training__body">
        <div class="training__info-body" v-if="!isTraining">
          <h2 class="training__info-title">
            Blind typing training web application.<br/>
            Implemented using the Vue framework.<br/>
            Click 'Start training' to run.<br/>
          </h2>
        </div>

        <div class="training__start-body" v-else>

        </div>
      </div>
    </div>

  </div>
</template>

<script>
import StartButton from "@/components/StartButton";

export default {
  name: 'App',
  components: {
    StartButton,
  },
  data() {
    return {
      isTraining: false,
    }
  },
  methods: {
    changeIsTraining() {
      this.isTraining = !this.isTraining;
    }
  },
  computed: {

  }
}
</script>

<style>
  * {
    background-color: white;
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    border: none;
  }

  .app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
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

    border: 1px solid black;
    border-radius: 5px;
  }

  .training__body {
    max-width: 800px;
    height: 700px;
    margin: 0 auto;
    padding: 10px 30px;

    display: flex;
    flex-direction: column;
    justify-content: center;

    border: 1px solid black;
    border-radius: 5px;
  }

  .training__info-body  {
    position: relative;
    max-width: 75%;
    margin: 0 auto 30%;
    padding: 0 10px;
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
  }

  .training__start-body {
    width: 100%;
    height: 100%;
    border: 1px solid black;
  }
</style>
