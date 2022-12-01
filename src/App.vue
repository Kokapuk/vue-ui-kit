<template>
  <div class="h-100 container" :class="theme">
    <!-- <button class="button me-10">Click Me</button> -->
    <Button @click="() => (buttonLoading = !buttonLoading)" :loading="buttonLoading">Hello</Button>
    <Button disabled="true" :loading="false">Can't click me</Button>

    <div class="seperator"></div>

    <input @change="(event) => {theme = (event.target as HTMLInputElement).checked ? 'theme-light' : 'theme-dark'}" class="switch me-10" type="checkbox" />
    <input disabled class="switch me-10" type="checkbox" />
    <input disabled checked class="switch me-10" type="checkbox" />

    <div class="seperator"></div>

    <input name="radio" class="radio me-10" type="radio" />
    <input name="radio" class="radio me-10" type="radio" />
    <input disabled name="radio-2" class="radio me-10" type="radio" />
    <input disabled checked name="radio-2" class="radio me-10" type="radio" />

    <div class="seperator"></div>

    <input placeholder="Input some text" type="text" class="text-input me-10" />
    <input placeholder="Input some password" type="password" class="text-input me-10" />
    <input disabled placeholder="You can't input data" type="text" class="text-input me-10" />

    <div class="seperator"></div>

    <Slider v-model="percentage" />
    <span>{{ `${percentage}%` }}</span>

    <div class="seperator"></div>

    <input class="checkbox me-10" type="checkbox" />
    <input class="checkbox me-10" type="checkbox" disabled />
    <input class="checkbox me-10" type="checkbox" disabled checked />

    <div class="seperator"></div>

    <div class="loading-indicator"></div>

    <div class="seperator"></div>

    <SelectMenu />
    <div class="seperator"></div>
    <SelectMenu />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import Slider from './components/Slider.vue';
import Button from './components/Button.vue';
import SelectMenu from './components/SelectMenu.vue';

const percentage = ref(10);
const theme = ref<'theme-dark' | 'theme-light'>('theme-dark');

const buttonLoading = ref(false);
</script>

<style scoped>
.container {
  padding: 50px 250px;
  background-color: rgb(var(--bg-color));
  color: rgb(var(--font-color));
}
</style>
