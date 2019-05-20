<template>
  <div class="counters-container">
    <div class="counter">
      <h2>Score:</h2>
      <h3>{{ score }}</h3>
    </div>
    <div class="counter">
      <h2>High Score:</h2>
      <h3>{{ highScore }}</h3>
    </div>
    <div class="counter">
      <h2>Timer:</h2>
      <h3>{{ time }}</h3>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    score: Number,
    highScore: Number,
    time: Number,
  },
}
</script>

<style>
.counters-container {
  display: flex;
  justify-content: space-evenly;
}

.counter {
  border: 1px solid #000;
  margin-top: 20px;
  padding: 20px;
}

.counter h1,
.counter h2 {
  margin: 0;
}
</style>
