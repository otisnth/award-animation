<template>
  <AnimationBase
    animationClass="bubble"
    :createContent="createBubbleContent"
    :intervalRange="[300, 500]"
    :duration="4000"
  >
    <slot />
  </AnimationBase>
</template>

<script setup lang="ts">
import AnimationBase from "./AnimationBase.vue";

const createBubbleContent = () => {
  const size = Math.random() * 20 + 10; // Размер пузыря от 10 до 30px
  return `
    <div style="
      width: ${size}px;
      height: ${size}px;
      background-color: rgba(173, 216, 230, 0.5);
      border-radius: 50%;
    "></div>
  `;
};
</script>

<style>
.bubble {
  position: absolute;
  pointer-events: none;
  animation: bubble-rise 4s ease-out forwards;
}

@keyframes bubble-rise {
  0% {
    transform: translateY(0) scale(1);
    opacity: 1;
  }
  100% {
    transform: translateY(-200px) scale(0.8);
    opacity: 0;
  }
}
</style>
