<template>
  <div
    @blur="() => emit('close')"
    ref="menuContainer"
    tabindex="0"
    :class="['menu-container', { shown: props.show }]"
    :style="{ left: clampHorizontal(props.offsetX) + 'px', top: clampVertical(props.offsetY) + 'px' }">
    <Button v-for="item in props.menuItems" @click="item.clickHandle" :disabled="item.disabled" noRipples class="menu-item">
      {{ item.text }}
    </Button>

    <!-- <Button noRipples class="menu-item">Open</Button>
    <Button noRipples class="menu-item">Open with...</Button>
    <Button noRipples class="menu-item">Copy</Button>
    <Button noRipples class="menu-item">Cut</Button>
    <Button noRipples class="menu-item">Send by email as text or image</Button>
    <Button noRipples class="menu-item">Add to storage</Button> -->
  </div>
</template>

<script setup lang="ts">
export interface IMenuItem {
  text: string;
  disabled?: boolean;
  clickHandle?: (event: MouseEvent) => void;
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
const emit = defineEmits(['close']);
const menuContainer = ref<null | HTMLDivElement>(null);

watch(
  () => props.show,
  () => {
    if (props.show) menuContainer.value!.focus();
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
</script>

<style scoped>
.menu-container {
  pointer-events: none;
  position: absolute;
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
</style>
