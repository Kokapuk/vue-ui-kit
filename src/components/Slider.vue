<template>
  <div @mousedown="(event) => (mouseDown = event.button === 0)" @click="(event) => percentageUpdate(event)" ref="slider" class="slider mb-5">
    <div class="filled-zone" :style="{ width: props.modelValue + '%' }">
      <div class="filled-circle"></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const props = defineProps(['modelValue']);
const emit = defineEmits(['update:modelValue']);
const mouseDown = ref(false);
const slider = ref<null | HTMLDivElement>(null);

function clampPercentage(value: number): number {
  value = Math.floor(value);

  if (value <= 0) return 0;
  else if (value >= 100) return 100;
  else return value;
}

function percentageUpdate(event: MouseEvent) {
  emit('update:modelValue', clampPercentage((event.pageX - slider.value!.offsetLeft) / (slider.value!.clientWidth / 100)));
}

onmouseup = (event) => (mouseDown.value = !(event.button === 0));
onmousemove = (event) => {
  if (!mouseDown.value) return;
  percentageUpdate(event);
};
</script>

<style scoped>
.slider {
  background-color: rgb(var(--light-gray));
  height: 10px;
  width: 100%;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color var(--transition-duration);
}

.filled-zone {
  position: relative;
  background-color: rgb(var(--accent));
  height: 100%;
  width: 50%;
  border-radius: 5px;
}

.filled-circle {
  position: absolute;
  background-color: white;
  aspect-ratio: 1 / 1;
  height: 150%;
  top: -25%;
  right: -7.5px;
  border-radius: 50%;
  box-shadow: var(--shadow);
}
</style>
