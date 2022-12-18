<template>
  <div ref="container" class="menu-container" style="position: relative" tabindex="0">
    <button :disabled="props.disabled">
      {{ selectedOption.text }}
      <Chevron class="chevron" />
    </button>
    <div v-if="!props.disabled" class="menu">
      <button v-for="option in props.options" :disabled="option.disabled" @click="() => updateValue(option)" class="menu-item">
        {{ option.text }}
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
export interface IOption {
  text: string;
  value: string;
  disabled?: boolean;
}

import { ref, type PropType } from 'vue';
import Chevron from './Icons/Chevron.vue';

const props = defineProps({ options: Array<IOption>, selectedOption: Object as PropType<IOption>, disabled: Boolean });
const emit = defineEmits(['update:selectedOption']);
const selectedOption = ref<IOption>(props.selectedOption!);
const container = ref<null | HTMLDivElement>(null);

function updateValue(newValue: IOption) {
  selectedOption.value = newValue;
  emit('update:selectedOption', newValue);

  container.value!.focus();
  container.value!.blur();
}
</script>

<style scoped>
::-webkit-scrollbar {
  width: 7px;
  background-color: rgb(var(--light-gray));
  border-bottom-right-radius: var(--border-radius);
}

::-webkit-scrollbar-thumb {
  background-color: rgb(var(--font-color), 0.2);
  border-radius: var(--border-radius);
}

button {
  position: relative;
  display: flex;
  justify-content: space-between;
  overflow: hidden;
  border: 0;
  background-color: rgb(var(--light-gray));
  border-radius: var(--border-radius);
  width: 100%;
  font-size: 17px;
  padding: 10px 15px;
  text-align: left;
  align-items: center;
  cursor: pointer;
  transition: all var(--transition-duration);
}

button:disabled {
  cursor: not-allowed;
}

button:not(:disabled)::before {
  content: '';
  position: absolute;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgb(var(--font-color));
  opacity: 0;
  transition: var(--transition);
}

button:hover::before {
  opacity: 0.1;
}

.chevron {
  transition: var(--transition);
}

button:disabled .chevron {
  opacity: 0.5;
}

.menu-container:focus-within .chevron {
  rotate: -180deg;
}

.menu-container:focus-within button {
  border-bottom-left-radius: 0;
  border-bottom-right-radius: 0;
}

.menu {
  position: absolute;
  width: 100%;
  z-index: 1;
  border-bottom-left-radius: var(--border-radius);
  border-bottom-right-radius: var(--border-radius);
  max-height: 0px;
  overflow: auto;
  transition: var(--transition);
}

.menu-container:focus-within .menu {
  max-height: 200px;
}

.menu-item {
  border-radius: 0;
  border-top: 1px solid rgb(var(--font-color), 0.1);
}
</style>
