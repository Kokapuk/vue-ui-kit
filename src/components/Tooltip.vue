<template>
  <div class="tooltip">
    <slot />
    <span class="tooltip-text">
      {{ props.text }}
      <div class="tooltip-arrow"></div>
    </span>
  </div>
</template>

<script setup lang="ts">
interface IProps {
  text: string;
}

const props = defineProps<IProps>();
</script>

<style scoped>
.tooltip {
  --tooltip__arrow-size: 10px;
  --tooltip__border-opacity: 0.1;
  --tooltip__border-width: 1px;

  position: relative;
  display: inline-flex;
  justify-content: center;
}

.tooltip-text {
  width: max-content;
  background-color: rgb(var(--light-gray));
  border: var(--tooltip__border-width) solid rgb(var(--font-color), var(--tooltip__border-opacity));
  color: rgv(var(--font-color));
  padding: 5px 10px;
  border-radius: var(--border-radius);
  position: absolute;
  z-index: 1;
  bottom: 125%;
  opacity: 0;
  scale: 0.9;
  pointer-events: none;
  transition: opacity var(--transition-duration), scale var(--transition-duration), background-color var(--transition-duration);
}

.tooltip-arrow {
  content: '';
  z-index: -1;
  position: absolute;
  width: var(--tooltip__arrow-size);
  height: var(--tooltip__arrow-size);
  background-color: rgb(var(--light-gray));
  bottom: calc(-1 * (var(--tooltip__arrow-size) / 2));
  left: calc(50% - (var(--tooltip__arrow-size) / 2));
  border-right: var(--tooltip__border-width) solid rgb(var(--font-color), var(--tooltip__border-opacity));
  border-bottom: var(--tooltip__border-width) solid rgb(var(--font-color), var(--tooltip__border-opacity));
  rotate: 45deg;
  transition: background-color var(--transition-duration);
}

.tooltip:hover .tooltip-text {
  opacity: 1;
  scale: 1;
}
</style>
