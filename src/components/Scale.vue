<template>
  <div class="scale">
    <div class="scale-rail">
      <div class="scale-rail__fill"></div>
      <div class="check-button-group">
        <div
          v-for="option, index in options"
          :key="index"
          :class="['check-button', { active: option.value === value }]"
          @click="onUpdateValue(option.value)"
        >{{ option }}</div>
      </div>
    </div>
  </div>
</template>

<script setup>

const emit = defineEmits(['update:value'])
defineProps({
  value: Number,
  options: Object,
})

const onUpdateValue = (value) => {
  emit('update:value', value)
}
</script>

<style lang="scss">
.scale {
  --a-scale-height: 12px;
  --a-check-button-size: 36px;
  width: 100%;
  padding: calc((var(--a-check-button-size) - var(--a-scale-height)) / 2) calc(var(--a-check-button-size) / 2);
}

.scale-rail {
  height: var(--a-scale-height);
  background-color: #dbdbdf;
  position: relative;
}

.check-button-group {
  display: flex;
  justify-content: space-between;
  position: absolute;
  top: 50%;
  left: 0;
  right: 0;
  margin-left: calc(var(--a-check-button-size) / 2 * -1);;
  margin-right: calc(var(--a-check-button-size) / 2 * -1);;
  transform: translateY(-50%);
}

.check-button {
  width: var(--a-check-button-size);
  height: var(--a-check-button-size);
  border-radius: calc(var(--a-check-button-size) / 2);
  border: 2px solid #2080f0;
  background-color: #fff;
  &.active {
    background-color: #2080f0;
  }
}
</style>