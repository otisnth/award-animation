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
    | "frozen";
}

const { animation } = defineProps<Props>();

const AnimationComponent = defineAsyncComponent(() => {
  switch (animation) {
    case "sparks":
      return import("./animations/AnimationSparks.vue");

    case "stars":
      return import("./animations/AnimationStars.vue");

    case "confetti":
      return import("./animations/AnimationConfetti.vue");

    case "bubbles":
      return import("./animations/AnimationBubbles.vue");

    case "hearts":
      return import("./animations/AnimationHearts.vue");

    case "fireworks":
      return import("./animations/AnimationFireworks.vue");

    case "flames":
      return import("./animations/AnimationFlames.vue");

    case "rainbow":
      return import("./animations/AnimationRainbow.vue");

    case "holographic":
      return import("./animations/AnimationHolographic.vue");

    case "fog":
      return import("./animations/AnimationFog.vue");

    case "frozen":
      return import("./animations/AnimationFrozen.vue");
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
