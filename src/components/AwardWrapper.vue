<template>
  <component :is="AnimationComponent" class="animation-wrapper">
    <slot />
  </component>
</template>

<script setup lang="ts">
import { defineAsyncComponent } from "vue";

interface Props {
  animation:
    | "stars"
    | "sparks"
    | "confetti"
    | "bubbles"
    | "hearts"
    | "fireworks"
    | "flames"
    | "rainbow"
    | "holographic"
    | "fog"
    | "frozen"
    | "ufo"
    | "divine"
    | "shimmer"
    | "pulse";
}

const { animation } = defineProps<Props>();

const AnimationComponent = defineAsyncComponent(() => {
  try {
    return import(`./animations/Animation${animation.charAt(0).toUpperCase() + animation.slice(1)}.vue`);
  } catch (e) {
    return Promise.resolve();
  }
});
</script>

<style>
.animation-wrapper {
  position: relative;
  display: inline-block;
  overflow: visible;
}
</style>
