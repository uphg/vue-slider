<template>
  <div>
    <Slider v-model:value="a" :step="5" :min="1" :max="100"/>
    {{ a }}
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import Slider from './components/Slider.vue'

const a = ref(2)
</script>