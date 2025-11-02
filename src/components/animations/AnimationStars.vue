<template>
  <AnimationBase
    animationClass="award-star"
    :createContent="createStarContent"
    :intervalRange="[400, 600]"
    :duration="2000"
  >
    <slot />
  </AnimationBase>
</template>

<script setup lang="ts">
import AnimationBase from "./AnimationBase.vue";

const createStarContent = () => `
  <svg viewBox="0 0 24 24" width="30" height="30" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
    <defs>
      <filter id="glow" x="-50%" y="-50%" width="200%" height="200%">
        <feGaussianBlur stdDeviation="2" result="coloredBlur"/>
        <feMerge>
          <feMergeNode in="coloredBlur"/>
          <feMergeNode in="SourceGraphic"/>
        </feMerge>
      </filter>
      <radialGradient id="starGradient" cx="50%" cy="50%" r="70%">
        <stop offset="0%" stop-color="#ffe600ff" stop-opacity="1"/>
        <stop offset="100%" stop-color="gold" stop-opacity="0.8"/>
      </radialGradient>
    </defs>
    <polygon points="12,4 11,12 13,12" fill="url(#starGradient)" filter="url(#glow)" />
    <polygon points="12,20 11,12 13,12" fill="url(#starGradient)" filter="url(#glow)" />
    <polygon points="4,12 12,11 12,13" fill="url(#starGradient)" filter="url(#glow)" />
    <polygon points="20,12 12,11 12,13" fill="url(#starGradient)" filter="url(#glow)" />
  </svg>
`;
</script>

<style>
.award-star {
  position: absolute;
  pointer-events: none;
  opacity: 0;
  animation: star-twinkle 2s ease-out forwards;
}

@keyframes star-twinkle {
  0% {
    transform: scale(0) rotate(0deg);
    opacity: 0;
  }
  20% {
    transform: scale(1.2) rotate(20deg);
    opacity: 1;
  }
  50% {
    transform: scale(1) rotate(0deg);
    opacity: 1;
  }
  100% {
    transform: scale(0.5) rotate(-20deg);
    opacity: 0;
  }
}
</style>
