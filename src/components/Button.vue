<template>
  <button :disabled="props.loading" class="button me-10">
    <Transition name="fade">
      <div v-if="props.loading" class="loading-container">
        <div class="loading-indicator" />
      </div>
    </Transition>
    <slot class="me-5" />
  </button>
</template>

<script setup lang="ts">
const props = defineProps({ loading: Boolean });
</script>

<style scoped>
.button {
  background-color: rgb(var(--accent));
  padding: 10px 15px;
  height: 40px;
  color: white;
  border-radius: var(--border-radius);
  font-size: 17px;
  border: 0;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}

.button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.button::after {
  content: '';
  position: absolute;
  background-color: black;
  opacity: 0;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  transition: var(--transition);
}

.button:hover::after {
  opacity: 0.2;
}

.button:active::after {
  opacity: 0.4;
}

.loading-container {
  display: flex;
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: inherit;
  justify-content: center;
  align-items: center;
}

.loading-indicator {
  border-color: white;
}

.loading-indicator {
  height: 70%;
  border-width: 5px;
}

.fade-enter-active,
.fade-leave-active {
  transition: var(--transition);
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
