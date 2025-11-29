<template>
  <div class="frozen-container">
    <div class="slot-content" ref="content">
      <slot />
    </div>

    <div class="ice-shell" ref="shell">
      <div class="ice-frost"></div>
      <div class="ice-frost ice-frost--secondary"></div>

      <!-- <div class="cracks">
        <span class="crack crack-1"></span>
        <span class="crack crack-2"></span>
        <span class="crack crack-3"></span>
        <span class="crack crack-4"></span>
      </div> -->

      <!-- <div class="ice-shards">
        <span
          v-for="shard in shards"
          :key="shard.id"
          class="ice-shard"
          :style="{
            '--delay': `${shard.delay}s`,
            '--angle': `${shard.angle}deg`,
            '--tx': `${shard.tx}%`,
            '--ty': `${shard.ty}%`,
            '--scale': shard.scale
          }"
        ></span>
      </div> -->
    </div>
  </div>
</template>

<script setup lang="ts">
import { nextTick, onMounted, useTemplateRef } from "vue";

const shards = Array.from({ length: 10 }, (_, index) => ({
  id: index,
  delay: 1.6 + index * 0.08,
  angle: -30 + index * 8,
  tx: -15 + index * 4,
  ty: index % 2 === 0 ? -25 + index * 2 : 20 - index * 1.5,
  scale: 0.8 + (index % 3) * 0.1,
}));

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
  width: 100%;
  height: 100%;
  display: grid;
  place-items: center;
  overflow: hidden;
  isolation: isolate;
}

.slot-content {
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
  background: url("@/assets/img/ice.svg") 10% 10% / cover no-repeat,
    radial-gradient(circle at 40% 30%, rgba(255, 255, 255, 0.9), rgba(120, 217, 255, 0.7) 45%, rgba(14, 72, 99, 0.8)),
    linear-gradient(145deg, rgba(197, 233, 255, 0.8), rgba(86, 162, 201, 0.7));
  mix-blend-mode: screen;
  opacity: 0;
  border-radius: 18%;
  filter: blur(2px);
  animation: frost-grow 5.2s ease infinite forwards;
}

.ice-frost--secondary {
  inset: -15%;
  background: conic-gradient(from 45deg, rgba(255, 255, 255, 0.3), rgba(26, 119, 150, 0.6), rgba(255, 255, 255, 0.3));
  opacity: 0;
  animation: frost-grow-secondary 5s ease infinite forwards 0.2s;
}

.cracks {
  position: absolute;
  inset: 10%;
  opacity: 0;
  animation: cracks-reveal 2.8s ease forwards 0.9s;
}

.crack {
  position: absolute;
  display: block;
  border-top: 1px solid rgba(255, 255, 255, 0.9);
  border-bottom: 1px solid rgba(130, 216, 255, 0.5);
  width: 70%;
  transform-origin: left center;
  filter: drop-shadow(0 0 6px rgba(187, 236, 255, 0.6));
}

.crack-1 {
  top: 40%;
  left: 15%;
  transform: rotate(-8deg);
}

.crack-2 {
  top: 55%;
  left: 10%;
  width: 80%;
  transform: rotate(12deg);
}

.crack-3 {
  top: 35%;
  left: 30%;
  width: 40%;
  transform: rotate(45deg);
}

.crack-4 {
  top: 60%;
  left: 35%;
  width: 35%;
  transform: rotate(-35deg);
}

.ice-shards {
  position: absolute;
  inset: 0;
  overflow: visible;
}

.ice-shard {
  position: absolute;
  top: 50%;
  left: 50%;
  width: clamp(16px, 4vw, 42px);
  height: clamp(8px, 2vw, 22px);
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.95), rgba(131, 210, 255, 0.9));
  border-radius: 999px;
  opacity: 0;
  transform-origin: center;
  filter: drop-shadow(0 0 6px rgba(140, 208, 255, 0.8));
  animation: shard-flight 1.8s ease-in forwards;
  animation-delay: var(--delay);
}

@keyframes frost-grow {
  0% {
    opacity: 0;
  }
  40% {
    opacity: 0.85;
  }
  70% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

@keyframes frost-grow-secondary {
  0% {
    opacity: 0;
  }
  50% {
    opacity: 0.5;
  }
  100% {
    opacity: 0;
  }
}

@keyframes cracks-reveal {
  0% {
    opacity: 0;
  }
  40% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

@keyframes shard-flight {
  0% {
    opacity: 0;
    transform: translate(-50%, -50%) scale(0.2);
  }
  20% {
    opacity: 1;
    transform: translate(-50%, -50%) scale(1);
  }
  100% {
    opacity: 0;
    transform: translate(calc(-50% + var(--tx)), calc(-50% + var(--ty))) rotate(var(--angle)) scale(var(--scale));
  }
}
</style>
