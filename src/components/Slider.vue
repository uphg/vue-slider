<template>
  <div class="a-slider" @click="onClick">
    <div class="a-slider-rail">
      <div class="a-slider-rail__fill" :style="{ width: `${a}%` }"></div>
      <div class="a-slider-handles" ref="handles">
        <div
          class="a-slider-handle-wrapper"
          ref="handleWrapper"
          :style="{ left: `${proportion}%` }"
          @pointerdown="onPointerdown"
        >
          <div class="a-slider-handle"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, shallowRef, computed, onUnmounted } from 'vue-demi';

const props = defineProps({
  min: {
    type: Number,
    default: 0
  },
  max: {
    type: Number,
    default: 100
  },
  step: {
    type: Number,
    default: 1
  },
  onUpdateValue: Function
})

const handleWrapper = shallowRef(null)
const handles = shallowRef(null)
const stepSize = ref(0)

const stepRatio = computed(() => props.step / props.max)
const proportion = computed(() => {
  const progress = ((props.value * stepSize.value) / handles.value?.offsetWidth ?? 0) * 100
  const proportion = progress < 0 ? 0 : progress > 100 ? 100 : progress
  return proportion
})

let pressed = null

const onPointerdown = () => {
  pressed = true
}

const onPointermove = (event) => {
  event.preventDefault();
  if (!pressed) return
  triggerSlider(event)
}

const onPointerup = () => {
  if (!pressed) return
  pressed = null
}

const onClick = (event) => {
  triggerSlider(event)
}

const triggerSlider = (event) => {
  const { offsetWidth: handlesWidth } = handles.value
  const rect = handles.value.getBoundingClientRect()
  const positionX = event.clientX - rect.left
  stepSize.value = stepRatio.value * handlesWidth
  const percent = positionX / stepSize.value
  const stepCount = round(percent)
  // const progress = ((stepCount * stepSize.value) / handlesWidth) * 100
  // const proportion = progress < 0 ? 0 : progress > 100 ? 100 : progress
  props.onUpdateValue?.(stepCount)
}

onMounted(() => {
  document.addEventListener('pointermove', onPointermove)
  document.addEventListener('pointerup', onPointerup)
})

onUnmounted(() => {
  document.removeEventListener('pointermove', onPointermove)
  document.removeEventListener('pointerup', onPointerup)
})


function round(number) {
  const integer = Math.floor(number);
  const decimalPart = number - integer;
  return decimalPart * 10 >= 5 ? integer + props.step : integer
}
</script>

<style scoped>
.a-slider {
  --a-rail-height: 12px;
  --a-handle-size: 36px;
  box-sizing: border-box;
  cursor: pointer;
  width: 100%;
  padding: calc((var(--a-handle-size) - var(--a-rail-height)) / 2) calc(var(--a-handle-size) / 2);
  position: relative;
}

.a-slider-rail {
  position: relative;
  background-color: #dbdbdf;
  height: var(--a-rail-height);
  border-radius: calc(var(--a-rail-height) / 2)
}

.a-slider-rail__fill {
  height: 100%;
  width: 20%;
  background-color: #2080f0;
  border-radius: calc(var(--a-rail-height) / 2);
}

.a-slider-handles {
  position: absolute;
  inset: 0;
}

.a-slider-handle-wrapper {
  width: var(--a-handle-size);
  height: var(--a-handle-size);
  position: absolute;
  top: 50%;
  transform: translate(-50%, -50%);
  cursor: pointer;
}

.a-slider-handle {
  width: 100%;
  height: 100%;
  border: 2px solid #2080f0;
  border-radius: calc(var(--a-handle-size) / 2);
  background-color: #fff;
}
</style>