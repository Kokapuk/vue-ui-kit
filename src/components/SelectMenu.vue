<template>
  <div ref="container" @blur="() => (menuShow = false)" style="position: relative" tabindex="0">
    <button :disabled="props.disabled" :class="{ 'menu-shown': menuShow }" @click="() => (menuShow = !menuShow)">
      {{ selectedOption.text }}
      <Chevron class="chevron" :class="{ 'menu-shown': menuShow }" />
    </button>
    <div v-if="!props.disabled" :class="{ 'menu-shown': menuShow }" class="menu">
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

import { ref, watch, type PropType } from 'vue';
import Chevron from './Icons/Chevron.vue';

const props = defineProps({ options: Array<IOption>, selectedOption: Object as PropType<IOption>, disabled: Boolean });
const emit = defineEmits(['update:selectedOption']);
const menuShow = ref(false);
const container = ref<null | HTMLDivElement>(null);
const selectedOption = ref<IOption>(props.selectedOption!);

watch(
  () => menuShow.value,
  (newValue) => {
    if (newValue) container.value!.focus();
  }
);

function updateValue(newValue: IOption) {
  selectedOption.value = newValue;
  emit('update:selectedOption', newValue);
}
</script>

<style scoped>
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

.chevron.menu-shown {
  rotate: -180deg;
}

button.menu-shown {
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
  overflow-y: auto;
  transition: var(--transition);
}

.menu.menu-shown {
  max-height: 300px;
}

.menu-item {
  border-radius: 0;
  border-top: 1px solid rgb(var(--font-color), 0.1);
}
</style>
