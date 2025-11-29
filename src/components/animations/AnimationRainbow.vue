<template>
  <div class="rainbow-container" ref="rainbowContainer" :style="{ '--parent-bg-color': parentBackgroundColor }">
    <div class="rainbow" ref="rainbowEl">
      <div class="stars-container" ref="starsContainer"></div>
      <div class="rainbow-arc red">
        <div class="rainbow-arc orange">
          <div class="rainbow-arc yellow">
            <div class="rainbow-arc green">
              <div class="rainbow-arc blue">
                <div class="rainbow-arc indigo">
                  <div class="rainbow-arc purple"></div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <img class="cloud left" ref="leftCloud" src="@/assets/img/cloud.svg" />
    <img class="cloud right" ref="rightCloud" src="@/assets/img/cloud.svg" />
    <div class="content">
      <slot />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, useTemplateRef } from "vue";

const rainbowEl = ref(null);
const leftCloud = ref(null);
const rightCloud = ref(null);

const starsContainer = useTemplateRef("starsContainer");
const rainbowContainer = useTemplateRef("rainbowContainer");
const parentBackgroundColor = ref("#ffffff"); // значение по умолчанию

// Функция для поиска непрозрачного фона в родительских элементах
function findParentBackgroundColor(element) {
  if (!element) return "#ffffff";
  
  let current = element.parentElement;
  
  while (current) {
    const bgColor = getComputedStyle(current).backgroundColor;
    
    // Проверяем, является ли цвет непрозрачным
    if (bgColor && bgColor !== "rgba(0, 0, 0, 0)" && bgColor !== "transparent") {
        return bgColor;
    }
    
    // Останавливаемся на body или html
    if (current.tagName === "BODY" || current.tagName === "HTML") {
      break;
    }
    
    current = current.parentElement;
  }
  
  return "#ffffff"; // значение по умолчанию
}

// Тайминги (в миллисекундах) — при необходимости подкорректируйте
const LEFT_DELAY = 1500; // левое облако появляется через 1.5s
const RAINBOW_DELAY = 3000; // радуга появляется через 3s
const HIDE_AFTER_RAINBOW = 4500; // через сколько после появления радуги начнётся её скрытие
const HIDE_STEP = 700; // интервал между скрытиями элементов (радуга -> левое -> правое)
const CYCLE = RAINBOW_DELAY + HIDE_AFTER_RAINBOW + HIDE_STEP * 2 + 4500; // общий интервал цикла

let cycleTimer = null;
let leftTimer = null;
let rainbowTimer = null;
let hideRainbowTimer = null;
let hideLeftTimer = null;
let hideRightTimer = null;
let cleanupTimer = null;

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

let intervalStars;

function clearTimers() {
  clearTimeout(leftTimer);
  clearTimeout(rainbowTimer);
  clearTimeout(hideRainbowTimer);
  clearTimeout(hideLeftTimer);
  clearTimeout(hideRightTimer);
  clearTimeout(cleanupTimer);
  clearInterval(intervalStars);
}

function resetClasses() {
  [rightCloud.value, leftCloud.value, rainbowEl.value].forEach((el) => {
    if (!el) return;
    el.classList.remove("show");
    el.classList.remove("hide");
  });
}

function playOnce() {
  // Сброс перед стартом
  resetClasses();
  
  // Очищаем предыдущий интервал звезд, если он существует
  clearInterval(intervalStars);

  // Небольшая задержка для гарантии перерисовки
  setTimeout(() => {
    // Появление: правое -> левое -> радуга
    if (rightCloud.value) rightCloud.value.classList.add("show");

    leftTimer = setTimeout(() => {
      if (leftCloud.value) leftCloud.value.classList.add("show");
      intervalStars = setInterval(createAnimationElement, 100 + Math.random() * 50);
    }, LEFT_DELAY);

    rainbowTimer = setTimeout(() => {
      if (rainbowEl.value) rainbowEl.value.classList.add("show");
    }, RAINBOW_DELAY);

    // Скрытие в обратном порядке: радуга -> левое -> правое
    hideRainbowTimer = setTimeout(() => {
      if (rainbowEl.value) rainbowEl.value.classList.add("hide");
    }, RAINBOW_DELAY + HIDE_AFTER_RAINBOW);

    hideLeftTimer = setTimeout(() => {
      if (rightCloud.value) rightCloud.value.classList.add("hide");
      clearInterval(intervalStars);
    }, RAINBOW_DELAY + HIDE_AFTER_RAINBOW + 4000);

    hideRightTimer = setTimeout(() => {
      if (leftCloud.value) leftCloud.value.classList.add("hide");
    }, RAINBOW_DELAY + HIDE_AFTER_RAINBOW + HIDE_STEP + 4000);

    // Убираем классы полностью чтобы можно было повторить цикл
    cleanupTimer = setTimeout(() => {
      resetClasses();
    }, RAINBOW_DELAY + HIDE_AFTER_RAINBOW + HIDE_STEP + 4000 + 1600);
  }, 20);
}

onMounted(() => {
  // Получаем цвет фона первого непрозрачного родительского элемента
  if (rainbowContainer.value) {
    parentBackgroundColor.value = findParentBackgroundColor(rainbowContainer.value);
  }

  playOnce();
  cycleTimer = setInterval(() => {
    clearTimers();
    playOnce();
  }, CYCLE);
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
.rainbow-container {
background-color: inherit;
}

.content {
  position: relative;
}

.stars-container {
  position: absolute;
  left: 20%;
  height: 15%;
  width: 60%;
}

.rainbow {
  background-color: transparent;
  position: absolute;
  width: 340%;
  height: 550%;
  top: -55%;
  left: -120%;
  display: flex;
  justify-content: center;
  align-items: stretch;
}

.rainbow::after {
  position: absolute;
  left: 0;
  content: "";
  width: 100%;
  height: 200px;
  background: var(--parent-bg-color, #ffffff);
}

.rainbow.show::after {
  animation: show-rainbow 6000ms ease forwards;
  animation-fill-mode: both;
}

.rainbow.hide::after {
  animation: hide-rainbow 4000ms ease forwards;
  animation-fill-mode: both;
}


.rainbow-arc {
  background-color: transparent;
  display: flex;
  flex: 1;
  border-radius: 50%;
  border: 10px solid transparent;
}

.red {
  border-top-color: tomato;
}

.orange {
  border-top-color: orange;
}

.yellow {
  border-top-color: yellow;
}

.green {
  border-top-color: lawngreen;
}

.blue {
  border-top-color: deepskyblue;
}

.indigo {
  border-top-color: mediumslateblue;
}

.purple {
  border-top-color: mediumorchid;
}

/* Clouds */

.cloud {
  width: 150px;
  height: 120px;
  position: absolute;
  left: 70%;
  top: 60%;
  margin-top: -120px;
  opacity: 0; /* скрываем по умолчанию */
}

.cloud.left {
  z-index: 3;
  margin-left: -240px;
}

.cloud.right {
  z-index: 3;
  margin-left: 45px;
}

.cloud.show {
  animation: show-cloud 600ms ease forwards;
  animation-fill-mode: both;
}

/* скрытие облака */
.cloud.hide {
  animation: hide-cloud 700ms ease forwards;
  animation-fill-mode: both;
}

@keyframes show-cloud {
  0% {
    opacity: 0;
  }

  100% {
    opacity: 1;
  }
}

@keyframes hide-cloud {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}

@keyframes show-rainbow {
  0% {
    left: 0;
    width: 100%;
  }
  100% {
    width: 0;
  }
}

@keyframes hide-rainbow {
  from {
    left: 100%;
    width: 0;
  }
  to {
    width: 100%;
  }
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
