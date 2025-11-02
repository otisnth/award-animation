<template>
  <AnimationBase
    animationClass="award-spark"
    :createContent="createSparkContent"
    :intervalRange="[150, 350]"
    :duration="1800"
  >
    <slot />
  </AnimationBase>
</template>

<script setup lang="ts">
import AnimationBase from "./AnimationBase.vue";

const createSparkContent = () => {
  const colors = ["#ffd700", "#ff8c00", "#47ec1eff", "#ff4500", "#e94feeff", "#5c5ffaff", "#ffffff"];
  const color = colors[Math.floor(Math.random() * colors.length)];
  return `
    <svg viewBox="0 0 24 24" width="15" height="15" xmlns="http://www.w3.org/2000/svg">
      <defs>
        <radialGradient id="sparkGradient" cx="50%" cy="50%" r="60%">
          <stop offset="0%" stop-color="${color}" stop-opacity="1"/>
          <stop offset="60%" stop-color="${color}" stop-opacity="0.7"/>
          <stop offset="100%" stop-color="transparent" stop-opacity="0"/>
        </radialGradient>
      </defs>
      <path d="M 12 6 L 14 12 L 20 12 L 14 16 L 12 22 L 10 16 L 4 16 L 10 12 L 8 6 Z" fill="url(#sparkGradient)" />
    </svg>
  `;
};
</script>

<style>
.award-spark {
  position: absolute;
  pointer-events: none;
  animation: sparkFly 1.8s ease-out forwards;
}

@keyframes sparkFly {
  0% {
    transform: translate(0, 0) scale(1);
    opacity: 0;
  }
  10% {
    opacity: 1;
  }
  90% {
    opacity: 0.3;
  }
  100% {
    transform: translate(0px, -25px) scale(1.5);
    opacity: 0;
  }
}
</style>
