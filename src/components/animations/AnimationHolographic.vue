<template>
  <div
    ref="el"
    class="transform-container"
  >
    <div class="transform-content" ref="content">
      <slot />
    </div>
    <div class="transform-gloss" ref="gloss" />
  </div>
</template>

<script setup lang="ts">
import { onMounted, onBeforeUnmount, ref, nextTick } from "vue";

const el = ref<HTMLElement | null>(null);
const gloss = ref<HTMLElement | null>(null);
const content = ref<HTMLElement | null>(null);
let styleEl: HTMLStyleElement | null = null;

function onMove(e: MouseEvent | { clientX: number; clientY: number }) {
  if (!gloss.value || !el.value) return;

  const clientX = (e as any).clientX;
  const clientY = (e as any).clientY;

  const l = clientX;
  const t = clientY;
  const h = window.innerHeight;
  const w = window.innerWidth;  

  if (w === 0 || h === 0) return;

  const lPercent = 100 * (l / w);
  const tPercent = 100 * (t / h);

  const px = Math.abs(Math.floor(lPercent) - 100);
  const py = Math.abs(Math.floor(tPercent) - 100);
  
  const pa = 50 - px + (50 - py);
  
  const lp = 50 + (px - 50) / 1.5;
  const tp = 50 + (py - 50) / 1.5;
  const px_spark = 50 + (px - 50) / 7;
  const py_spark = 50 + (py - 50) / 7;
  const p_opc = 20 + Math.abs(pa) * 1.5;
  const ty = ((tp - 50) / 2) * -1.5;
  const tx = ((lp - 50) / 1.5) * 1.5;

  const grad_pos = `background-position: ${lp}% ${tp}%;`;
  const sprk_pos = `background-position: ${px_spark}% ${py_spark}%;`;
  const opc = `opacity: ${p_opc / 100};`;
  const tf = `perspective(150px) rotateX(${ty}deg) rotateY(${tx}deg)`;

  (el.value.style as any).transform = tf;

  gloss.value.classList.add("active");

  const css = `
    .transform-gloss.active:before { ${grad_pos} }
    .transform-gloss.active:after { ${sprk_pos} ${opc} }
  `;
  if (styleEl) styleEl.innerHTML = css;
}

function onTouchMove(e: TouchEvent) {
  if (!e.touches || e.touches.length === 0) return;
  e.preventDefault();
  const t = e.touches[0]!;
  onMove({ clientX: t.clientX, clientY: t.clientY });
}

function setMaskImageFromSlot() {
  if (!content.value || !gloss.value) return;

  const img = content.value.querySelector("img") as HTMLImageElement | null;
  if (img) {
    const updateMask = () => {
      const imgSrc = img.currentSrc || img.src || img.getAttribute("src");
      if (imgSrc) {
        const maskUrl = `url("${imgSrc}")`;
        gloss.value!.style.setProperty("--mask-image", maskUrl);
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
  styleEl = document.createElement("style");
  styleEl.id = "transform-hover-style";
  document.head.appendChild(styleEl);

  nextTick(() => {
    setMaskImageFromSlot();
  });

  document.body.addEventListener("mousemove", onMove, { passive: true });
  document.body.addEventListener("touchmove", onTouchMove, { passive: false });
});

onBeforeUnmount(() => {
  document.body.removeEventListener("mousemove", onMove);
  document.body.removeEventListener("touchmove", onTouchMove);
  
  if (styleEl && styleEl.parentNode) styleEl.parentNode.removeChild(styleEl);
});
</script>

<style>
* {
  --color1: #efb2fb;
  --color2: #acc6f8;
}

.transform-container {
  position: relative;
  z-index: 10;
  touch-action: none;
  isolation: isolate;
}

.transform-content {
  position: relative;
  z-index: 1;
}

.transform-content :is(img) {
  display: block;
  filter: drop-shadow(-10px -10px 10px var(--color1)) drop-shadow(10px 10px 10px var(--color2));
  transition: filter 0.2s ease;
}

.transform-gloss {
  position: absolute;
  inset: 0;
  border-radius: inherit;
  pointer-events: none;
  mix-blend-mode: screen;

  overflow: hidden;
  z-index: 10;
  touch-action: none;
  isolation: isolate;

  transition: transform 0.5s ease, box-shadow 0.2s ease;
  will-change: transform, filter;

  background-size: cover;
  background-repeat: no-repeat;
  background-position: 50% 50%;
  transform-origin: center;
}


.transform-gloss:before,
.transform-gloss:after {
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

.transform-gloss:before,
.transform-gloss:after {
  content: "";
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  background-repeat: no-repeat;
  opacity: 0.5;
  mix-blend-mode: color-dodge;
  transition: all 0.33s ease;
}

.transform-gloss:before {
  background-position: 50% 50%;
  background-size: 300% 300%;
  background-image: linear-gradient(
    115deg,
    transparent 0%,
    var(--color1) 25%,
    transparent 47%,
    transparent 53%,
    var(--color2) 75%,
    transparent 100%
  );
  opacity: 0.5;
  filter: brightness(0.5) contrast(1);
  z-index: 1;
}

.transform-gloss:after {
  opacity: 1;
  background-image: url("@/assets/img/holographic/sparkles.webp"), url("@/assets/img/holographic/holo.webp"),
    linear-gradient(125deg, #ff008450 15%, #fca40040 30%, #ffff0030 40%, #00ff8a20 60%, #00cfff40 70%, #cc4cfa50 85%);
  background-position: 50% 50%;
  background-size: 160%;
  background-blend-mode: overlay;
  z-index: 2;
  filter: brightness(1) contrast(1);
  transition: all 0.33s ease;
  mix-blend-mode: color-dodge;
  opacity: 0.75;
}

.transform-gloss.active:after,
.transform-gloss:hover:after {
  filter: brightness(1) contrast(1);
  opacity: 1;
}

.transform-gloss.active,
.transform-gloss:hover {
  animation: none;
  transition: box-shadow 0.1s ease-out;
}

.transform-gloss.active:before,
.transform-gloss:hover:before {
  animation: none;
  background-image: linear-gradient(110deg, transparent 25%, var(--color1) 48%, var(--color2) 52%, transparent 75%);
  background-position: 50% 50%;
  background-size: 250% 250%;
  opacity: 0.88;
  filter: brightness(0.66) contrast(1.33);
  transition: none;
}

.transform-gloss.active:before,
.transform-gloss:hover:before,
.transform-gloss.active:after,
.transform-gloss:hover:after {
  animation: none;
  transition: none;
}
</style>
