<script setup>
import Slider from './components/Slider.vue'
import Scale from './components/Scale.vue'
</script>

<template>
  <div id="app">
    <!-- <Slider v-model:value="value" :min="1" :max="10"/> -->
    <Slider/>
    <Scale/>
  </div>
</template>

<style scoped>
#app {
  width: 500px;
  display: flex;
  flex-direction: column;
}
</style>
