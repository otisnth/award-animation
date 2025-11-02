<template>
  <div ref="sparks-container">
    <slot />
  </div>
</template>

<script setup lang="ts">
import { onMounted, useTemplateRef } from "vue";

const awardRef = useTemplateRef("sparks-container");

onMounted(() => {
  if (awardRef.value) {
    const createSpark = () => {
      const spark = document.createElement("div");
      spark.className = "award-spark";

      // Случайное направление (угол)
      const angle = Math.random() * Math.PI * 2;
      const duration = 1000 + Math.random() * 800; // время жизни

      const colors = ["#ffd700", "#ff8c00", "#47ec1eff", "#ff4500", "#e94feeff", "#5c5ffaff", "#ffffff"];
      const color = colors[Math.floor(Math.random() * colors.length)];

      const path = `
      <svg viewBox="0 0 24 24" width="15" height="15" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <radialGradient id="sparkGradient" cx="50%" cy="50%" r="60%">
            <stop offset="0%" stop-color="${color}" stop-opacity="1"/>
            <stop offset="60%" stop-color="${color}" stop-opacity="0.7"/>
            <stop offset="100%" stop-color="transparent" stop-opacity="0"/>
          </radialGradient>
        </defs>
        <path d="
            M 12 6
            L 14 12 L 20 12 L 14 16 L 12 22 L 10 16 L 4 16 L 10 12 L 8 6
            Z
        "
        fill="url(#sparkGradient)" />
      </svg>
    `;

      spark.innerHTML = path;

      // Позиция — случайная точка внутри элемента
      const rect = awardRef.value?.getBoundingClientRect();
      const x = Math.random() * rect.width;
      const y = Math.random() * rect.height;

      spark.style.position = "absolute";
      spark.style.left = `${x}px`;
      spark.style.top = `${y}px`;
      spark.style.transformOrigin = "center center";
      spark.style.opacity = "0";
      spark.style.pointerEvents = "none";

      // Анимация: вылет в направлении угла
      spark.style.transition = `transform ${duration}ms ease-out, opacity ${duration}ms ease-out`;
      spark.style.transform = `translate(${Math.cos(angle) * 100}px, ${Math.sin(angle) * 100}px)`;
      spark.style.opacity = "1";

      // Добавляем к элементу
      awardRef.value?.appendChild(spark);

      // Удаляем после анимации
      setTimeout(() => {
        spark.remove();
      }, duration);
    };

    // Запускаем искры каждые 150–350 мс
    const interval = setInterval(createSpark, 150 + Math.random() * 200);
  }
});
</script>

<style>
.award-spark {
  position: absolute;
  pointer-events: none;
  animation: sparkFly 1s ease-out forwards;
}

@keyframes sparkFly {
  0% {
    transform: translate(0, 0) scale(1);
    opacity: 0;
  }
  10% {
    opacity: 1;
  }
  90% {
    opacity: 0.3;
  }
  100% {
    transform: translate(0px, -25px) scale(1.5);
    opacity: 0;
  }
}
</style>
