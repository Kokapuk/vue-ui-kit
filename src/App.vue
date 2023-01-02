<template>
  <div class="h-100 container" :class="theme">
    <Tooltip class="me-10" text="Loading button" :positioning="TooltipPositioning.left">
      <Button @click="() => (buttonLoading = !buttonLoading)" noRipples :loading="buttonLoading"> Loading Button </Button>
    </Tooltip>
    <Button class="me-10">Ripples Button</Button>
    <Button class="me-10" noRipples>No Ripples Button</Button>
    <Button class="me-10" disabled>Can't click me</Button>
    <Button class="icon-button me-10"> <Microphone class="icon-button__icon" /> Icon button </Button>
    <Tooltip text="Icon button without text" :positioning="TooltipPositioning.right">
      <Button class="icon-button"> <Microphone class="icon-button__icon" /></Button>
    </Tooltip>

    <div class="seperator"></div>

    <Tooltip text="Theme switcher" :positioning="TooltipPositioning.left" class="me-10">
      <input
        @change="(event) => {theme = (event.target as HTMLInputElement).checked ? 'theme-light' : 'theme-dark'}"
        class="switch"
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

    <LoadingIndicator />

    <div class="seperator"></div>

    <SelectMenu
      :options="[
        { text: 'One', value: '1' },
        { text: 'Two', value: '2' },
        { text: 'Three', value: '3', disabled: true },
        { text: 'Four', value: '4' },
        { text: 'Five', value: '5' },
        { text: 'Six', value: '6' },
        { text: 'Seven', value: '7' },
      ]"
      v-model:selectedOption="option" />
    <div class="seperator"></div>
    <span>{{ `Value selected via select menu: ${option.value}` }}</span>
    <div class="seperator"></div>
    <SelectMenu disabled :options="[]" :selectedOption="{ text: 'Four', value: 'f' }" />

    <div class="seperator"></div>

    <Tooltip text="Top Tooltip Text" :positioning="TooltipPositioning.top" class="me-10">
      <Button disabled>Top Tooltip</Button>
    </Tooltip>
    <Tooltip text="Right Tooltip Text" :positioning="TooltipPositioning.right" class="me-10">
      <Button disabled>Right Tooltip</Button>
    </Tooltip>
    <Tooltip text="Bottom Tooltip Text" :positioning="TooltipPositioning.bottom" class="me-10">
      <Button disabled>Bottom Tooltip</Button>
    </Tooltip>
    <Tooltip text="Left Tooltip Text" :positioning="TooltipPositioning.left">
      <Button disabled>Left Tooltip</Button>
    </Tooltip>

    <div class="seperator"></div>

    <div class="center-y">
      <ProgressBar class="me-10" :progress="progressBarProgress" />
      <span style="width: 50px; text-align: right">{{ progressBarProgress }}%</span>
    </div>

    <div class="seperator"></div>

    <Button @click="() => progressBarProgress < 100 && (progressBarProgress += 10)" class="me-10">+ 10</Button>
    <Button @click="() => progressBarProgress > 0 && (progressBarProgress -= 10)">- 10</Button>

    <div class="seperator"></div>

    <ContextMenu
      @close="() => (showContextMenu = false)"
      :show="showContextMenu"
      :offset-x="contextMenuOffest.x"
      :offset-y="contextMenuOffest.y"
      :menu-items="[
        {
          text: 'Show in explorer',
          clickHandle: () => {
            log(1);
          },
        },
        {
          text: 'Rename',
          disabled: true,
          clickHandle: () => {
            log(2);
          },
        },
        {
          text: 'Unmute',
          icon: Microphone,
          separator: true,
          clickHandle: () => {
            log(3);
          },
        },
        {
          text: 'Delete',
          danger: true,
          clickHandle: () => {
            log(4);
          },
        },
      ]" />
    <Button @click="showContextMenuClickHandle">Show context menu</Button>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import Slider from './components/Slider.vue';
import Button from './components/Button.vue';
import Tooltip from './components/Tooltip.vue';
import SelectMenu, { type IOption } from './components/SelectMenu.vue';
import ProgressBar from './components/ProgressBar.vue';
import LoadingIndicator from './components/LoadingIndicator.vue';
import Microphone from './components/Icons/Microphone.vue';
import { TooltipPositioning } from './types';
import ContextMenu from './components/ContextMenu.vue';

const theme = ref<'theme-dark' | 'theme-light'>('theme-dark');
const percentage = ref(50);
const option = ref<IOption>({ text: 'One', value: '1' });
const buttonLoading = ref(false);
const progressBarProgress = ref(50);
const showContextMenu = ref(false);
const contextMenuOffest = ref({ x: 0, y: 0 });

function showContextMenuClickHandle(event: MouseEvent) {
  showContextMenu.value = true;
  contextMenuOffest.value.x = event.clientX;
  contextMenuOffest.value.y = event.clientY;
}

function log(content: any) {
  console.log(content);
}
</script>

<style scoped>
.container {
  padding: 50px 250px;
  background-color: rgb(var(--bg-color));
  color: rgb(var(--font-color));
  transition: background-color var(--transition-duration);
}
</style>
