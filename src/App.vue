<template>
  <div class="h-100 container" :class="theme">
    <Button class="me-10" @click="() => (buttonLoading = !buttonLoading)" noRipples :loading="buttonLoading">
      Loading Button
    </Button>
    <Button class="me-10">Ripples Button</Button>
    <Button class="me-10" noRipples>No Ripples Button</Button>
    <Button class="me-10" disabled>Can't click me</Button>
    <Button class="icon-button me-10"> <Microphone class="icon-button__icon" /> Icon button </Button>
    <Tooltip text="Icon button without text">
      <Button class="icon-button"> <Microphone class="icon-button__icon" /></Button>
    </Tooltip>

    <div class="seperator"></div>

    <Tooltip text="Theme switcher">
      <input
        @change="(event) => {theme = (event.target as HTMLInputElement).checked ? 'theme-light' : 'theme-dark'}"
        class="switch me-10"
        type="checkbox" />
    </Tooltip>
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

    <Slider v-model:value="percentage" :min="0" :max="100" :step="1" />
    <div class="seperator"></div>
    <span>{{ `${percentage}%` }}</span>

    <div class="seperator"></div>

    <input class="checkbox me-10" type="checkbox" />
    <input class="checkbox me-10" type="checkbox" disabled />
    <input class="checkbox me-10" type="checkbox" disabled checked />

    <div class="seperator"></div>

    <div class="loading-indicator"></div>

    <div class="seperator"></div>

    <SelectMenu
      :options="[
        { text: 'One', value: 'o' },
        { text: 'Two', value: 't' },
        { text: 'Three', value: 'th', disabled: true },
      ]"
      v-model:selectedOption="option" />
    <div class="seperator"></div>
    <span>{{ `Value selected via select menu: ${option.value}` }}</span>
    <div class="seperator"></div>
    <SelectMenu disabled :options="[]" :selectedOption="{ text: 'Four', value: 'f' }" />

    <div class="seperator"></div>

    <Tooltip text="Tooltip text example">
      <Button disabled>Tooltip</Button>
    </Tooltip>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import Slider from './components/Slider.vue';
import Button from './components/Button.vue';
import Tooltip from './components/Tooltip.vue';
import SelectMenu, { type IOption } from './components/SelectMenu.vue';
import Microphone from './components/Icons/Microphone.vue';

const theme = ref<'theme-dark' | 'theme-light'>('theme-dark');
const percentage = ref(50);
const option = ref<IOption>({ text: 'One', value: 'o' });
const buttonLoading = ref(false);
</script>

<style scoped>
.container {
  padding: 50px 250px;
  background-color: rgb(var(--bg-color));
  color: rgb(var(--font-color));
  transition: background-color var(--transition-duration);
}
</style>
