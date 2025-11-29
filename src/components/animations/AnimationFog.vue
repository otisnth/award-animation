<template>
  <div
    ref="el"
    class="fog-container"
    @mousemove.prevent="onMove"
    @touchmove.passive="onTouchMove"
    @mouseenter.prevent="onEnter"
    @mouseleave="onLeave"
  >
    <div class="fogs" ref="fogs">
      <img class="fog-1" src="@/assets/img/fog/fog6.png" alt="" />
      <img class="fog-2" src="@/assets/img/fog/fog6.png" alt="" />
    </div>
    <slot />
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref, useTemplateRef } from "vue";

const el = ref<HTMLElement | null>(null);

const fogs = useTemplateRef("fogs");

function setMaskPosition(px: number, py: number) {
  const node = fogs.value;
  if (!node) return;
  node.style.setProperty("--mx", px * 100 + 5 + "%");
  node.style.setProperty("--my", py * 100 - 5 + "%");
}

function onMove(e: MouseEvent) {
  if (!fogs.value) return;
  const rect = fogs.value.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const y = e.clientY - rect.top;
  const px = x / rect.width;
  const py = y / rect.height;

  setMaskPosition(px, py);
}

function onTouchMove(e: TouchEvent) {
  if (!e.touches || e.touches.length === 0) return;
  const t = e.touches[0]!;
  onMove({ clientX: t.clientX, clientY: t.clientY } as MouseEvent);
}

function onEnter() {
  if (!fogs.value) return;

  fogs.value.style.setProperty("--mx", "50%");
  fogs.value.style.setProperty("--my", "50%");
  fogs.value.style.setProperty(
    "--mask-image",
    "radial-gradient(circle at var(--mx) var(--my), transparent 0%, black 10%)"
  );
}

function onLeave() {
  if (fogs.value) {
    fogs.value.style.setProperty("--mx", "50%");
    fogs.value.style.setProperty("--my", "50%");
    fogs.value.style.removeProperty("--mask-image");
  }
}
</script>

<style scoped>
.fog-container {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: visible;
}

.fogs {
  --mx: 50%;
  --my: 50%;
  --mask-size: 100%;

  cursor: url("@/assets/img/flashlight.svg") 0 0, pointer;


  position: absolute;

  overflow: visible;
  width: 500%;
  height: 150%;
  top: -25%;
  left: -200%;
  /* z-index: 3; */


  -webkit-mask-image: var(--mask-image);
  mask-image: var(--mask-image);

  -webkit-mask-size: var(--mask-size);
  mask-size: var(--mask-size);
  -webkit-mask-repeat: no-repeat;
  mask-repeat: no-repeat;
  -webkit-mask-position: var(--mx) var(--my);
  mask-position: var(--mx) var(--my);
  -webkit-mask-mode: alpha;
  mask-mode: alpha;
}

.fog-1 {
  display: block;
  /* z-index: 3; */
  position: absolute;
  top: 0;
  left: 25%;

  width: 50%;
  height: 100%;

  overflow: visible;

  animation: fog 8000ms linear infinite;
}

.fog-2 {
  display: block;
  /* z-index: 3; */
  position: absolute;
  top: 0;
  left: 25%;

  width: 50%;
  height: 100%;
  opacity: 0;

  overflow: visible;

  transform: rotate(180deg);

  animation: fog 8000ms linear infinite 4000ms;
}

@keyframes fog {
  0% {
    left: 20%;
    opacity: 0;
  }
  20% {
    opacity: 1;
  }
  80% {
    opacity: 1;
  }
  100% {
    opacity: 0;
    left: 30%;
  }
}
</style>
