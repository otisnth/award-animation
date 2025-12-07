<template>
  <div class="divine-container">
    <slot />
  </div>
</template>

<style scoped>
.divine-container {
  position: relative;
  z-index: 0;
  overflow: visible;
}

.divine-container::before {
  content: "";
  position: absolute;
  inset: -10%;
  border-radius: 50%;
  background: repeating-conic-gradient(from 0deg, rgba(255, 215, 0, 0.6) 0deg 6deg, transparent 6deg 18deg);
  mix-blend-mode: screen;
  transform: scale(0.2);
  opacity: 0;
  animation: divine-burst 1s ease-out infinite;
  z-index: -1;
}

@keyframes divine-burst {
  0% {
    transform: scale(0.2);
    rotate: 0deg;
    opacity: 0;
  }
  10% {
    transform: scale(0.5);
    opacity: 1;
  }
  20% {
    transform: scale(0.7);
    opacity: 0.9;
  }
  40% {
    transform: scale(0.9);
    opacity: 0.8;
  }
  70% {
    transform: scale(1.2);
    opacity: 0.5;
  }
  100% {
    rotate: 90deg;
    transform: scale(1.5);
    opacity: 0;
  }
}
</style>
