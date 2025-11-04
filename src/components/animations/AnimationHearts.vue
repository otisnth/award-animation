<template>
  <AnimationBase
    animationClass="heart"
    :createContent="createHeartContent"
    :intervalRange="[500, 700]"
    :duration="2500"
  >
    <slot />
  </AnimationBase>
</template>

<script setup lang="ts">
import AnimationBase from "./AnimationBase.vue";

const createHeartContent = () => `
  <svg viewBox="0 0 24 24" width="20" height="20" xmlns="http://www.w3.org/2000/svg" fill="red">
    <path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"/>
  </svg>
`;
</script>

<style>
.heart {
  position: absolute;
  pointer-events: none;
  animation: heart-float 2.5s ease-out forwards;
}

@keyframes heart-float {
  0% {
    transform: translateY(0) scale(1);
    opacity: 1;
  }
  100% {
    transform: translateY(100%) scale(0.8);
    opacity: 0;
  }
}
</style>
