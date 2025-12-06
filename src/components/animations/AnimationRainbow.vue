<template>
  <div class="rainbow-container">
    <img class="rainbow-animation" src="@/assets/img/rainbow.svg" />
    <div class="stars-container" ref="starsContainer"></div>
    <slot />
  </div>
</template>

<script setup>
import { onMounted, onBeforeUnmount, useTemplateRef } from "vue";

const starsContainer = useTemplateRef("starsContainer");

let startTimers;
let endTimers;
let intervalStars;
let cycleTimer;

const createAnimationElement = () => {
  const element = document.createElement("div");
  element.className = "award-star";
  element.innerHTML = createStarContent();

  const rect = starsContainer.value?.getBoundingClientRect();
  if (!rect) return;
  element.style.left = `${Math.random() * rect.width}px`;
  element.style.top = `${Math.random() * rect.height}px`;

  starsContainer.value?.appendChild(element);

  setTimeout(() => {
    element.remove();
  }, 1500);
};

function clearTimers() {
  clearTimeout(startTimers);
  clearTimeout(endTimers);
  clearInterval(intervalStars);
}

function playOnce() {
  clearInterval(intervalStars);

  setTimeout(() => {
    startTimers = setTimeout(() => {
      intervalStars = setInterval(createAnimationElement, 100 + Math.random() * 50);
    }, 3000);

    endTimers = setTimeout(() => {
      clearInterval(intervalStars);
    }, 10000);
  }, 20);
}

onMounted(() => {
  playOnce();
  cycleTimer = setInterval(() => {
    clearTimers();
    playOnce();
  }, 17000);
});

onBeforeUnmount(() => {
  clearInterval(cycleTimer);
  clearInterval(intervalStars);
  clearTimers();
});

const size = Math.random() * 30 + 30;

const createStarContent = () => `
  <svg viewBox="0 0 24 24" width="${size}" height="${size}" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
    <defs>
      <filter id="glow" x="-50%" y="-50%" width="200%" height="200%">
        <feGaussianBlur stdDeviation="2" result="coloredBlur"/>
        <feMerge>
          <feMergeNode in="coloredBlur"/>
          <feMergeNode in="SourceGraphic"/>
        </feMerge>
      </filter>
      <radialGradient id="starWhiteGradient" cx="50%" cy="50%" r="70%">
        <stop offset="0%" stop-color="#ffffffff" stop-opacity="1"/>
        <stop offset="100%" stop-color="white" stop-opacity="0.8"/>
      </radialGradient>
    </defs>
    <polygon points="12,4 11,12 13,12" fill="url(#starWhiteGradient)" filter="url(#glow)" />
    <polygon points="12,20 11,12 13,12" fill="url(#starWhiteGradient)" filter="url(#glow)" />
    <polygon points="4,12 12,11 12,13" fill="url(#starWhiteGradient)" filter="url(#glow)" />
    <polygon points="20,12 12,11 12,13" fill="url(#starWhiteGradient)" filter="url(#glow)" />
  </svg>
`;
</script>

<style lang="css" scoped>
.stars-container {
  position: absolute;
  left: -30%;
  height: 60%;
  width: 160%;
  top: -30%;
}

.rainbow-animation {
  position: absolute;
  z-index: 1;
  width: 300%;
  height: 300%;
  top: -75%;
  left: -100%;
}

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
