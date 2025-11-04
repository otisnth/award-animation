<template>
  <AnimationBase
    animationClass="firework"
    :createContent="createFireworkContent"
    :intervalRange="[800, 1200]"
    :duration="2000"
  >
    <slot />
  </AnimationBase>
</template>

<script setup lang="ts">
import AnimationBase from "./AnimationBase.vue";

const createFireworkContent = () => `
  <svg viewBox="0 0 24 24" width="30" height="30" xmlns="http://www.w3.org/2000/svg">
    <circle cx="12" cy="12" r="2" fill="yellow" />
    <line x1="12" y1="0" x2="12" y2="6" stroke="red" stroke-width="2" />
    <line x1="12" y1="18" x2="12" y2="24" stroke="blue" stroke-width="2" />
    <line x1="0" y1="12" x2="6" y2="12" stroke="green" stroke-width="2" />
    <line x1="18" y1="12" x2="24" y2="12" stroke="purple" stroke-width="2" />
  </svg>
`;
</script>

<style>
.firework {
  position: absolute;
  pointer-events: none;
  animation: firework-explode 2s ease-out forwards;
}

@keyframes firework-explode {
  0% {
    transform: scale(0.5);
    opacity: 1;
  }
  50% {
    transform: scale(1.5);
    opacity: 1;
  }
  100% {
    transform: scale(2);
    opacity: 0;
  }
}
</style>
