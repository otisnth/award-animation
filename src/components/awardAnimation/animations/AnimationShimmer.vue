<template>
  <div class="shimmer-container" ref="shimmer">
    <slot />
  </div>
</template>

<script setup lang="ts">
import { nextTick, onMounted, useTemplateRef } from "vue";

const shimmer = useTemplateRef("shimmer");

function setMaskImageFromSlot() {
  if (!shimmer.value) return;

  const img = shimmer.value.querySelector("img") as HTMLImageElement | null;
  if (img) {
    const updateMask = () => {
      const imgSrc = img.currentSrc || img.src || img.getAttribute("src");
      if (imgSrc) {
        const maskUrl = `url("${imgSrc}")`;
        shimmer.value!.style.setProperty("--mask-image", maskUrl);
      }
    };

    if (img.complete && img.naturalHeight !== 0) {
      updateMask();
    } else {
      img.addEventListener("load", updateMask, { once: true });
      updateMask();
    }
  }
}

onMounted(() => {
  nextTick(() => {
    setMaskImageFromSlot();
  });
});
</script>

<style scoped>
.shimmer-container {
  position: relative;
  overflow: hidden;

  -webkit-mask-image: var(--mask-image);
  mask-image: var(--mask-image);
  -webkit-mask-size: 100% 100%;
  mask-size: 100% 100%;
  -webkit-mask-repeat: no-repeat;
  mask-repeat: no-repeat;
  -webkit-mask-position: center;
  mask-position: center;
  -webkit-mask-mode: alpha;
  mask-mode: alpha;
}

.shimmer-container::before {
  content: "";
  position: absolute;

  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.1),
    rgba(252, 235, 203, 0.4),
    rgba(250, 230, 193, 0.5),
    rgba(255, 255, 255, 0.1),
    transparent
  );
  mix-blend-mode: screen;

  animation: shimmer-slide 1.8s ease-in-out infinite;
  transform: skewX(-20deg);
  pointer-events: none;
}

@keyframes shimmer-slide {
  0% {
    left: -100%;
  }

  100% {
    left: 100%;
  }
}
</style>
