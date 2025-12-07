<template>
  <div class="pulse-container">
    <slot />
  </div>
</template>

<style scoped>
.pulse-container {
  animation: pulse-scale 1.5s infinite;
}

@keyframes pulse-scale {
  0%,
  100% {
    transform: scale(1);
  }

  50% {
    transform: scale(1.1);
  }
}
</style>
