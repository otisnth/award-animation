<template>
  <div class="fire-container">
    <img src="@/assets/img/fire.gif" class="fire" />

    <div v-for="i in COUNT_COAL" class="ember" aria-hidden="true" :style="getCoalStyles(i)">
      <span class="ember-core"></span>
      <img class="ember-crust" src="@/assets/img/coal.svg" />
      <div class="sparks" aria-hidden="true">
        <span></span>
        <span></span>
        <span></span>
        <span></span>
        <span></span>
      </div>
      <div class="ashes" aria-hidden="true">
        <span></span>
        <span></span>
        <span></span>
      </div>
      <span class="ember-smoke"></span>
    </div>

    <div class="fire-content">
      <slot />
    </div>
  </div>
</template>

<script setup lang="ts">
const COUNT_COAL = 5;

const getCoalStyles = (i) => {
  const size = Math.random() * 20 + 10;

  return {
    left: `${(100 / COUNT_COAL) * (i - 1)}%`,
    width: size + "px",
    height: size + "px",
    transform: `rotate(${Math.random() * 180 - 180}deg)`,
  };
};
</script>

<style lang="css" scoped>
.fire-container {
  position: relative;
}

.fire-content {
  position: relative;
  z-index: 4;
}

.fire {
  position: absolute;
  width: 130%;
  height: 130%;
  bottom: 10%;
  left: -25%;
}

.ember {
  position: absolute;
  transform: translateX(-50%);
  /* width: 40px;
  height: 30px; */
  pointer-events: none;
  z-index: 5; /* поверх фонового содержимого */
  bottom: 10%;
}

.ember-1 {
  left: 30%;
  bottom: 10%;
}

.ember-2 {
  left: 70%;
  bottom: 10%;
}

.ember-core {
  display: block;
  width: 70%;
  height: 100%;
  /* более сложная текстура: горячее ядро + тёмная обугленная корка (наслоение через отдельный элемент) */
  border-radius: 50% 50% 40% 40% / 60% 60% 40% 40%;
  background: radial-gradient(
    circle at 40% 30%,
    rgba(255, 230, 170, 1) 0%,
    #ffb450f2 25%,
    #dc5a1ecc 45%,
    rgba(120, 40, 10, 0.6) 65%,
    rgba(30, 12, 6, 1) 95%
  );
  box-shadow: 0 0 10px rgba(255, 150, 60, 0.5), inset 0 -2px 6px rgba(0, 0, 0, 0.45);
  filter: saturate(1.15) contrast(1.02);
  transform-origin: 50% 60%;
  animation: ember-flicker 1.4s infinite ease-in-out;
}

.ember-crust {
  position: absolute;
  left: 40%;
  transform: translateX(-50%);
  bottom: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  mix-blend-mode: multiply;
  opacity: 0.95;
}

.sparks {
  position: absolute;
  left: 50%;
  bottom: 90%;
  transform: translateX(-50%);
  width: 24px;
  height: 18px;
  pointer-events: none;
}
.sparks span {
  position: absolute;
  left: 50%;
  bottom: 0;
  width: 3px;
  height: 3px;
  border-radius: 50%;
  background: radial-gradient(circle, #ffdba0 0%, #ff7a00 50%, rgba(255, 120, 60, 0.25) 100%);
  box-shadow: 0 0 6px rgba(255, 140, 60, 0.9);
  opacity: 0;
  transform-origin: center bottom;
  animation: spark-rise 900ms ease-out infinite;
}
.sparks span:nth-child(1) {
  left: 20%;
  animation-delay: 0ms;
  transform: translateX(-50%) rotate(-8deg) scale(0.9);
}
.sparks span:nth-child(2) {
  left: 40%;
  animation-delay: 150ms;
  transform: translateX(-50%) rotate(6deg) scale(0.8);
}
.sparks span:nth-child(3) {
  left: 60%;
  animation-delay: 300ms;
  transform: translateX(-50%) rotate(-4deg) scale(0.75);
}
.sparks span:nth-child(4) {
  left: 80%;
  animation-delay: 420ms;
  transform: translateX(-50%) rotate(10deg) scale(0.9);
}
.sparks span:nth-child(5) {
  left: 50%;
  animation-delay: 260ms;
  transform: translateX(-50%) rotate(0deg) scale(1);
}

.ashes {
  position: absolute;
  left: 50%;
  bottom: 100%;
  transform: translateX(-50%);
  width: 14px;
  height: 14px;
  pointer-events: none;
}
.ashes span {
  position: absolute;
  left: 50%;
  bottom: 0;
  width: 2px;
  height: 2px;
  border-radius: 50%;
  background: rgba(150, 140, 140, 0.6);
  filter: blur(0.6px);
  opacity: 0;
  animation: ash-float 1600ms linear infinite;
}
.ashes span:nth-child(1) {
  left: 30%;
  animation-delay: 0ms;
}
.ashes span:nth-child(2) {
  left: 50%;
  animation-delay: 220ms;
}
.ashes span:nth-child(3) {
  left: 70%;
  animation-delay: 420ms;
}

.ember-smoke {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  bottom: 100%;
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: rgba(120, 100, 90, 0.18);
  filter: blur(2px);
  opacity: 0.9;
  animation: ember-smoke 2.2s infinite ease-out;
}

@keyframes ember-flicker {
  0% {
    transform: rotate(-1deg) scaleX(1) translateY(0);
    box-shadow: 0 0 6px rgba(255, 140, 0, 0.45), inset 0 -2px 6px rgba(0, 0, 0, 0.35);
  }
  20% {
    transform: rotate(1deg) scaleX(1.06) translateY(-1px);
    box-shadow: 0 0 12px rgba(255, 130, 20, 0.7), inset 0 -1px 6px rgba(0, 0, 0, 0.26);
  }
  40% {
    transform: rotate(-1deg) scaleX(0.98) translateY(0);
    box-shadow: 0 0 8px rgba(255, 100, 0, 0.55);
  }
  60% {
    transform: rotate(1deg) scaleX(1.03) translateY(-1px);
    box-shadow: 0 0 14px rgba(255, 150, 40, 0.8);
  }
  100% {
    transform: rotate(0deg) scaleX(1) translateY(0);
    box-shadow: 0 0 6px rgba(255, 140, 0, 0.5);
  }
}

@keyframes spark-rise {
  0% {
    transform: translateY(0) scale(0.8);
    opacity: 1;
    filter: blur(0px);
  }
  60% {
    transform: translateY(-12px) translateX(calc(-6px + var(--tx, 0px))) scale(0.7);
    opacity: 0.9;
    filter: blur(0.6px);
  }
  100% {
    transform: translateY(-26px) translateX(calc(-12px + var(--tx, 0px))) scale(0.5);
    opacity: 0;
    filter: blur(1.4px);
  }
}

@keyframes ash-float {
  0% {
    transform: translateY(0) scale(1);
    opacity: 0.7;
  }
  50% {
    transform: translateY(-10px) scale(0.9);
    opacity: 0.45;
  }
  100% {
    transform: translateY(-22px) scale(0.8);
    opacity: 0;
  }
}

@keyframes ember-smoke {
  0% {
    transform: translateX(-50%) translateY(0) scale(1);
    opacity: 0.9;
  }
  50% {
    transform: translateX(-50%) translateY(-6px) scale(1.3) rotate(-2deg);
    opacity: 0.45;
  }
  100% {
    transform: translateX(-50%) translateY(-14px) scale(1.6) rotate(2deg);
    opacity: 0;
  }
}

/* Slightly larger ember in compact screens but keep proportions */
@media (max-width: 420px) {
  .ember {
    width: 12px;
    height: 9px;
    bottom: 4px;
  }
  .sparks {
    width: 18px;
    height: 14px;
  }
}
</style>
