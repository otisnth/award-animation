<template>
  <div ref="el" class="ufo-container">
    <img class="ufo" src="@/assets/img/ufo.svg" ref="ufo" />

    <img class="ufo-ray" src="@/assets/img/ufoRay.svg" ref="ray" />

    <img class="ufo-grass" src="@/assets/img/ufoGrass.svg" />

    <div class="ufo-content" ref="award">
      <slot />
    </div>
  </div>
</template>

<script setup>
import { useTemplateRef, onMounted, onBeforeUnmount } from "vue";

const ufo = useTemplateRef("ufo");
const ray = useTemplateRef("ray");
const award = useTemplateRef("award");

const UFO_DELAY = 5000;
const UFO_RAY_DELAY = 4000;
const AWARD_DELAY = 4000;
const WAIT = 5000;
const CYCLE = (UFO_DELAY + UFO_RAY_DELAY + AWARD_DELAY) * 2 + WAIT;

let ufoRayDropTimer = null;
let awardDropTimer = null;
let awardUpTimer = null;
let ufoRayUpTimer = null;
let ufoOutTimer = null;
let cleanupTimer = null;
let cycleTimer = null;

function clearTimers() {
  clearTimeout(ufoRayDropTimer);
  clearTimeout(awardDropTimer);
  clearTimeout(awardUpTimer);
  clearTimeout(ufoRayUpTimer);
  clearTimeout(ufoOutTimer);
  clearTimeout(cleanupTimer);
}

function resetClasses() {
  [ufo.value, award.value, ray.value].forEach((el) => {
    if (!el) return;
    el.classList.remove("in");
    el.classList.remove("drop");
    el.classList.remove("up");
    el.classList.remove("out");
  });
}

function playOnce() {
  resetClasses();

  setTimeout(() => {
    if (ufo.value) ufo.value.classList.add("in");

    ufoRayDropTimer = setTimeout(() => {
      if (ray.value) ray.value.classList.add("drop");
    }, UFO_DELAY);

    awardDropTimer = setTimeout(() => {
      if (award.value) award.value.classList.add("drop");
    }, UFO_DELAY + UFO_RAY_DELAY);

    awardUpTimer = setTimeout(() => {
      if (award.value) award.value.classList.add("up");
    }, UFO_DELAY + UFO_RAY_DELAY + AWARD_DELAY + WAIT);

    ufoRayUpTimer = setTimeout(() => {
      if (ray.value) ray.value.classList.add("up");
    }, UFO_DELAY + UFO_RAY_DELAY + AWARD_DELAY * 2 + WAIT);

    ufoOutTimer = setTimeout(() => {
      if (ufo.value) ufo.value.classList.add("out");
    }, UFO_DELAY + UFO_RAY_DELAY * 2 + AWARD_DELAY * 2 + WAIT);

    cleanupTimer = setTimeout(() => {
      resetClasses();
    }, CYCLE);
  }, 20);
}

onMounted(() => {
  playOnce();
  cycleTimer = setInterval(() => {
    clearTimers();
    playOnce();
  }, CYCLE);
});

onBeforeUnmount(() => {
  clearInterval(cycleTimer);
  clearTimers();
});
</script>

<style scoped>
.ufo-container {
  position: relative;
  width: 150px;
  height: 150px;
}

.ufo {
  position: absolute;
  z-index: 2;
  left: -20%;
  width: 0px;
  aspect-ratio: 1;
}

.ufo.in {
  animation: ufo-fly 1s infinite, ufo-in 5s forwards;
}

.ufo.out {
  animation: ufo-fly 1s infinite, ufo-out 5s forwards;
}

@keyframes ufo-fly {
  0% {
    top: -90px;
  }
  50% {
    top: -85px;
  }
  100% {
    top: -90px;
  }
}

@keyframes ufo-in {
  0% {
    left: -20%;
    width: 0px;
  }
  100% {
    left: calc(50% - 32px);
    width: 64px;
  }
}

@keyframes ufo-out {
  0% {
    left: calc(50% - 32px);
    width: 64px;
  }
  100% {
    left: 120%;
    width: 0px;
  }
}

.ufo-content {
  position: absolute;
  transform: scale(0.25, 0.25);
  top: -50%;
  opacity: 0;
}

.ufo-content.drop {
  opacity: 1;
  animation: award-drop 4s forwards, award-move 5s infinite;
}

.ufo-content.up {
  animation: award-up 4s forwards;
}

@keyframes award-drop {
  0% {
    top: -100%;
    transform: scale(0.02, 0.02);
  }
  100% {
    top: 0;
    transform: scale(1, 1);
  }
}

@keyframes award-up {
  0% {
    top: 0;
    transform: scale(1, 1);
  }
  99% {
    top: -100%;
    transform: scale(0.02, 0.02);
  }
  100% {
    opacity: 0;
  }
}

@keyframes award-move {
  0% {
    rotate: y -30deg;
  }
  50% {
    rotate: y 30deg;
  }
  100% {
    rotate: y -30deg;
  }
}

.ufo-ray {
  position: absolute;
  z-index: 1;
  top: -45%;
  left: -50%;
  height: 0%;
  width: 200%;
  opacity: 0.5;
}

.ufo-ray.drop {
  animation: ray-drop 4s forwards;
}

.ufo-ray.up {
  animation: ray-up 4s forwards;
}

@keyframes ray-drop {
  0% {
    height: 0%;
  }
  100% {
    height: 180%;
  }
}

@keyframes ray-up {
  0% {
    height: 180%;
  }
  100% {
    height: 0%;
  }
}

.ufo-grass {
  position: absolute;
  width: 100%;
  left: 0%;
  bottom: -60%;
  clip-path: ellipse(50% farthest-side at top);
  perspective: 800px;
}
</style>
