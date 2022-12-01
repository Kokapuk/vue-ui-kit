<template>
  <div style="position: relative">
    <button :class="{ 'menu-shown': menuShow }" @click="() => (menuShow = !menuShow)">
      Select...
      <Chevron class="chevron" :class="{ 'menu-shown': menuShow }" />
    </button>
    <div ref="menu" @blur="() => (menuShow = false)" :class="{ 'menu-shown': menuShow }" class="menu" tabindex="0">
      <button class="menu-item">One</button>
      <button class="menu-item">Two</button>
      <button class="menu-item">Three</button>
      <button class="menu-item">Four</button>
      <button class="menu-item">Five</button>
      <button class="menu-item">Six</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue';
import Chevron from './Icons/Chevron.vue';

const menuShow = ref(false);
const menu = ref<null | HTMLDivElement>(null);

watch(
  () => {
    return menuShow.value;
  },
  (value) => {
    if (value) menu.value!.focus();
  }
);
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
  font-size: 20px;
  padding: 10px 15px;
  text-align: left;
  align-items: center;
  cursor: pointer;
}

button::before {
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
