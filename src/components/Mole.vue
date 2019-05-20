<template>
  <div
    class="mole-container"
    v-bind:class="{ active: this.isActive, inactive: !this.isActive }"
  >
    <div class="mole-image-container">
      <img
        v-on:click="$emit('strike')"
        class="mole"
        src="../assets/mole.png"
        alt="mole"
      />
    </div>
    <img class="dirt" src="../assets/dirt.svg" alt="mole dirt"/>
  </div>
</template>

<script>
export default {
  name: 'Mole',
  props: {
    isActive: Boolean,
  },
  methods: {
  },
}
</script>

<style>
.moles-container {
  margin-top: 20px;
  display: flex;
  justify-content: space-between;
  opacity: 0.5;
  transition: opacity 0.3s ease;
}

.moles-container.game-active {
  opacity: 1;
}

.mole-container {
  width: 160px;
  height: 160px;
  display: inline-block;
  margin: 10px;
  position: relative;
}

.mole-image-container {
  overflow: hidden;
  width: 160px;
  height: 140px;
}

.mole-container img {
  display: block;
  transition: all 0.3s ease;
}

.mole {
  position: relative;
  width: 60%;
  margin: auto;
  cursor: pointer;
}

.mole-container.active .mole {
  /* display: block; */
  top: 0px;
}

.mole-container.inactive .mole {
  top: 200px;
}

.dirt {
  width: 100%;
  margin: auto;
  z-index: 1;
  position: absolute;
  bottom: 0;
}
</style>
