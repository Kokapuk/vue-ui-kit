<template>
  <div
    @mousedown="(event) => (mouseDown = event.button === 0)"
    @click="(event) => percentageUpdate(event)"
    ref="slider"
    class="slider mb-5">
    <div class="filled-zone" :style="{ width: (100 / max) * props.value + '%' }">
      <div class="filled-circle"></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

interface IProps {
  value: number;
  min: number;
  max: number;
  step: number;
}

const props = defineProps<IProps>();
const emit = defineEmits(['update:value']);
const mouseDown = ref(false);
const slider = ref<null | HTMLDivElement>(null);

function clampPercentage(value: number): number {
  value = Number(value.toFixed(2));

  if (value <= props.min) return props.min;
  else if (value >= props.max) return props.max;
  else return value;
}

function percentageUpdate(event: MouseEvent) {
  let percentageDifference =
    props.value - clampPercentage((event.pageX - slider.value!.offsetLeft) / (slider.value!.clientWidth / props.max));
  if (Math.abs(percentageDifference) < props.step) return;

  let jump = Math.floor(Math.abs(percentageDifference) / props.step);
  emit(
    'update:value',
    clampPercentage(percentageDifference > 0 ? props.value - props.step * jump : props.value + props.step * jump)
  );
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
