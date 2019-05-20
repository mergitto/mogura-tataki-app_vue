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
    <div class="moles" v-bind:class="{ 'game-active': isGameActive }">
      <Mole
        v-for="(isMoleActive, index) in moles"
        v-bind:key="index"
        v-bind:isActive="isMoleActive"
        v-on:strike="handleStrike()"
      />
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
      limitSeconds: 15,
      time: 15,
      moles: [false, false, false, false],
      intervalId: Number,
      isGameActive: false,
    }
  },
  methods: {
    initializeGame: function() {
      this.time = this.limitSeconds,
      this.score = 0;
    },
    startGame: function() {
      this.initializeGame();
      this.startTimer();
      this.startMoles();
    },
    endGame: function() {
      this.stopTimer();
      this.stopMoles();
      this.setHighScore();
      this.moles = [false, false, false, false];
    },
    startTimer: function() {
      this.intervalId = setInterval(() => {
        this.activateRandomMole();
        this.time--;
        if (this.time <= 0) {
          this.endGame();
        }
      }, 1000);
    },
    stopTimer: function() {
      clearInterval(this.intervalId);
    },
    setHighScore: function() {
      if (this.highScore < this.score) {
        this.highScore = this.score;
      }
    },
    handleStrike: function() {
      this.score++;
    },
    startMoles: function() {
      this.isGameActive = true;
    },
    stopMoles: function() {
      this.isGameActive = false;
    },
    activateRandomMole: function() {
      this.moles[this.getRandomInt(0, 4)] = true;
    },
    getRandomInt: function(min, max) {
      return Math.floor(Math.random() * (max - min)) + min;
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

.moles {
  opacity: 0.5;
}

.moles.game-active {
  opacity: 1;
}
</style>
