<template>
  <AnimationBase
    animationClass="confetti"
    :createContent="createConfettiContent"
    :intervalRange="[100, 200]"
    :duration="2000"
  >
    <slot />
  </AnimationBase>
</template>

<script setup lang="ts">
import AnimationBase from "./AnimationBase.vue";

const createConfettiContent = () => {
  const colors = ["#ff0000", "#00ff00", "#0000ff", "#ffff00", "#ff00ff", "#00ffff"];
  const color = colors[Math.floor(Math.random() * colors.length)];
  return `
    <div style="
      width: 5px;
      height: 5px;
      background-color: ${color};
      transform: rotate(${Math.random() * 360}deg);
    "></div>
  `;
};
</script>

<style scoped>
:deep(.confetti) {
  position: absolute;
  pointer-events: none;
  animation: confetti-fall 3s ease-out forwards;
}

@keyframes confetti-fall {
  0% {
    transform: translateY(-50px) rotate(0deg);
    opacity: 1;
  }
  100% {
    transform: translateY(50px) rotate(360deg);
    opacity: 0;
  }
}
</style>
