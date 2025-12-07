<template>
  <component :is="AnimationComponent" class="animation-wrapper">
    <slot />
  </component>
</template>

<script setup lang="ts">
import { defineAsyncComponent } from "vue";

enum Animations {
  "stars" = "Звезды",
  "sparks" = "Искры",
  "confetti" = "Конфетти",
  "bubbles" = "Пузыри",
  "hearts" = "Сердца",
  "fireworks" = "Фейерверки",
  "flames" = "Огонь",
  "rainbow" = "Радуга",
  "holographic" = "Голографическая",
  "fog" = "Туман",
  "frozen" = "Заморозка",
  "ufo" = "Нло",
  "divine" = "Лучи",
  "shimmer" = "Блеск",
  "pulse" = "Пульс",
}

interface Props {
  animation: keyof typeof Animations;
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
  width: 150px;
  height: 150px;
}
</style>
