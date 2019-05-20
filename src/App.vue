<template>
  <div class="whackamole">
    <h1 class="logo">
      Whack-a-mole!
    </h1>
    <button
      class="start-game"
      v-on:click="startGame"
    >
      Start Game
    </button>
    <Counter
      v-bind:score="score"
      v-bind:highScore="highScore"
      v-bind:time="time"
    />
    <div class="moles-container gameActive">
      <Mole v-for="index in 4" v-bind:key="index" v-on:strike="handleStrike()" />
    </div>
  </div>
</template>

<script>
import Counter from './components/Counter';
import Mole from './components/Mole';

export default {
  name: 'App',
  components: {
    Counter,
    Mole,
  },
  data: function() {
    return {
      score: 0,
      highScore: 0,
      time: 15,
      isMoles: [false, false, false, false],
      intervalId: Number,
    }
  },
  methods: {
    startGame: function() {
      this.startTimer();
    },
    endGame: function() {
      this.stopTimer();
    },
    startTimer: function() {
      this.intervalId = setInterval(() => {
        this.time--;
        if (this.time <= 0) {
          this.stopTimer();
        }
      }, 1000);
    },
    stopTimer: function() {
      clearInterval(this.intervalId);
    },
    handleStrike: function() {
      this.score++;
    },
  },
};
</script>

<style>
.whackamole {
  font-family: 'Bungee', sans-serif;
  max-width: 960px;
  margin: auto;
  text-align: center;
}

.start-game {
  font-family: 'Bungee', sans-serif;
  padding: 20px;
  border-radius: 3px;
  border: 0;
  background-color: #52b1d6;
  color: #fff;
  font-size: 1em;
  cursor: pointer;
}
</style>
