<template>
  <div
    @blur="menuContainerBlurHandle"
    ref="menuContainer"
    tabindex="0"
    :class="['menu-container', { shown: props.show }]"
    :style="{ left: clampHorizontal(props.offsetX) + 'px', top: clampVertical(props.offsetY) + 'px' }">
    <Button
      @blur="menuItemBlurHandle"
      @focus="() => (hasFocus = true)"
      v-for="item in props.menuItems"
      @click="(event) => menuItemClickHandle(event, item)"
      :disabled="item.disabled"
      :class="['menu-item', item.separator && 'separator']">
      {{ item.text }}
    </Button>
  </div>
</template>

<script setup lang="ts">
export interface IMenuItem {
  text: string;
  disabled?: boolean;
  clickHandle?: (event: MouseEvent) => void;
  separator?: boolean;
}

import { ref, watch } from 'vue';
import Button from './Button.vue';

interface IProps {
  show?: boolean;
  offsetX: number;
  offsetY: number;
  menuItems: IMenuItem[];
}

const props = defineProps<IProps>();
const emit = defineEmits(['closeRequest']);
const menuContainer = ref<null | HTMLDivElement>(null);
const hasFocus = ref(false);

watch(
  () => props.show,
  () => {
    if (props.show) menuContainer.value!.focus();
    hasFocus.value = false;
  }
);

function clampHorizontal(x: number): number {
  if (menuContainer.value === null) return 0;

  if (x + menuContainer.value.clientWidth > window.innerWidth) {
    return window.innerWidth - menuContainer.value!.clientWidth;
  }

  return x;
}

function clampVertical(y: number) {
  if (menuContainer.value === null) return 0;

  if (y + menuContainer.value.clientHeight > window.innerHeight) {
    return window.innerHeight - menuContainer.value!.clientHeight;
  }

  return y;
}

function menuContainerBlurHandle() {
  setTimeout(() => {
    if (!hasFocus.value) emit('closeRequest');
  });
}

function menuItemClickHandle(event: MouseEvent, item: IMenuItem) {
  item.clickHandle && item.clickHandle(event);
  emit('closeRequest');
}

function menuItemBlurHandle() {
  hasFocus.value = false;
  menuContainerBlurHandle();
}
</script>

<style scoped>
.menu-container {
  z-index: 5;
  pointer-events: none;
  position: fixed;
  background-color: rgb(var(--light-gray));
  width: min-content;
  border-radius: var(--border-radius);
  overflow: hidden;
  opacity: 0;
  transition: opacity var(--transition-duration), left var(--transition-duration), top var(--transition-duration);
}

.menu-container.shown {
  pointer-events: all;
  opacity: 1;
}

.menu-item {
  width: 100%;
  justify-content: left;
  color: rgb(var(--font-color));
  background-color: transparent;
  border-radius: 0;
}

.menu-item.separator {
  border-bottom: 1px solid rgb(var(--font-color), 0.1);
}
</style>
