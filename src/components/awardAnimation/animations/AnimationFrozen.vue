<template>
  <div class="frozen-container">
    <div class="frozen-content" ref="content">
      <slot />
    </div>

    <div class="ice-shell" ref="shell">
      <div class="ice-frost"></div>
      <div class="ice-frost ice-frost--secondary"></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { nextTick, onMounted, useTemplateRef } from "vue";

const content = useTemplateRef("content");
const shell = useTemplateRef("shell");

function setMaskImageFromSlot() {
  if (!content.value || !shell.value) return;

  const img = content.value.querySelector("img") as HTMLImageElement | null;
  if (img) {
    const updateMask = () => {
      const imgSrc = img.currentSrc || img.src || img.getAttribute("src");
      if (imgSrc) {
        const maskUrl = `url("${imgSrc}")`;
        shell.value!.style.setProperty("--mask-image", maskUrl);
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
.frozen-container {
  position: relative;
  display: grid;
  place-items: center;
  overflow: hidden;
  isolation: isolate;
}

.frozen-content {
  position: relative;
  z-index: 1;
}

.ice-shell {
  position: absolute;
  inset: 0;
  pointer-events: none;
  z-index: 2;

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

.ice-frost {
  position: absolute;
  inset: -10%;
  background: url("@/assets/animations/frozen/ice.svg") center / cover no-repeat;

  /* radial-gradient(circle at 40% 30%, rgba(255, 255, 255, 0.9), rgba(120, 217, 255, 0.7) 45%, rgba(14, 72, 99, 0.8)),
    linear-gradient(145deg, rgba(197, 233, 255, 0.8), rgba(86, 162, 201, 0.7)) */
  mix-blend-mode: screen;
  opacity: 1;
  border-radius: 18%;
  filter: blur(1px);
  /* animation: frost-grow 5.2s ease infinite forwards; */
}

.ice-frost--secondary {
  inset: -15%;
  background: conic-gradient(from 45deg, rgba(255, 255, 255, 0.3), rgba(26, 119, 150, 0.6), rgba(255, 255, 255, 0.3));
  opacity: 0;
  /* animation: frost-grow-secondary 5s ease infinite forwards 0.2s; */
}
</style>
