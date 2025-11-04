<template>
  <div ref="animation-container">
    <slot />
  </div>
</template>

<script setup lang="ts">
import { onMounted, useTemplateRef } from "vue";

interface Props {
  animationClass: string;
  createContent: () => string;
  intervalRange?: [number, number];
  duration?: number;
}

const { animationClass, createContent, intervalRange = [400, 600], duration = 2000 } = defineProps<Props>();

const animationContainer = useTemplateRef("animation-container");

onMounted(() => {
  if (!animationContainer.value) {
    return;
  }

  const createAnimationElement = () => {
    const element = document.createElement("div");
    element.className = animationClass;
    element.innerHTML = createContent();

    const rect = animationContainer.value?.getBoundingClientRect();
    if (!rect) return;
    element.style.left = `${Math.random() * rect.width}px`;
    element.style.top = `${Math.random() * rect.height}px`;

    animationContainer.value?.appendChild(element);

    setTimeout(() => {
      element.remove();
    }, duration);
  };

  const interval = setInterval(
    createAnimationElement,
    intervalRange[0] + Math.random() * (intervalRange[1] - intervalRange[0])
  );

  return () => clearInterval(interval);
});
</script>
