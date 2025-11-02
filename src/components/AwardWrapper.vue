<template>
  <component :is="AnimationComponent" class="award-wrapper">
    <slot />
  </component>
</template>

<script setup lang="ts">
import { defineAsyncComponent } from "vue";

interface Props {
  animation: "stars" | "sparks";
}

const { animation } = defineProps<Props>();

const AnimationComponent = defineAsyncComponent(() => {
  switch (animation) {
    case "sparks":
      return import("./animations/AnimationSparks.vue");

    case "stars":
      return import("./animations/AnimationStars.vue");
  }
});
</script>

<style>
.award-wrapper {
  position: relative;
  display: inline-block;
  overflow: visible;
}
</style>
