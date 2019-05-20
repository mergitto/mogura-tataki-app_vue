<template>
  <div class="whackamole">
    <h1 class="logo">
      Whack-a-mole!
    </h1>
    <button
      class="start-game"
      v-on:click="startGame"
      v-bind:disabled="isGameActive"
    >
      Start Game
    </button>
    <Counter
      v-bind:score="score"
      v-bind:highScore="highScore"
      v-bind:time="time"
    />
    <div class="moles" v-bind:class="{ 'game-active': isGameActive }" v-on:click="handleClickCount()">
      <Mole
        v-for="(isMoleActive, index) in moles"
        v-bind:key="index"
        v-bind:isActive="isMoleActive"
        v-on:strike="handleStrike(index)"
      />
    </div>
    <div class="whackamole-footer">
      Average strike MOGURA: {{ this.clickAccuracyRate }} %
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
      allClickCount: 0,
      clickAccuracyRate: 0,
    }
  },
  methods: {
    initializeGame: function() {
      this.time = this.limitSeconds,
      this.score = 0;
      this.allClickCount = 0;
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
        if (this.activateMoleLength() >= 2) {
          this.inActivateMole();
        }
        this.activateRandomMole();

        if (this.allClickCount > 0) {
          this.clickAccuracyRate = Math.round((this.score / this.allClickCount) * 100, 2);
        }

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
    handleStrike: function(index) {
      this.score++;
      this.moles[index] = false;
    },
    handleClickCount: function() {
      this.allClickCount++;
    },
    startMoles: function() {
      this.isGameActive = true;
    },
    stopMoles: function() {
      this.isGameActive = false;
    },
    activateMoleLength: function() {
      return this.moles.filter(mole => mole).length;
    },
    activateRandomMole: function() {
      const randomInt = this.getRandomInt(0, 4);
      const activeMoleLength = this.activateMoleLength();
      if (activeMoleLength == this.moles.length) {
        return;
      }

      if (this.moles[randomInt]){
        this.activateRandomMole();
      } else {
        this.moles[randomInt] = true;
      }
    },
    inActivateMole: function() {
      const index = this.getRandomInt(0, 4);
      if (this.moles[index]) {
        this.moles[index] = false;
      } else {
        this.inActivateMole();
      }
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

.whackamole-footer {
  margin-top: 50px;
}
</style>
