<template>
  <div class="container">
    <AwardWrapper animation="sparks">
      <div class="award"></div>
    </AwardWrapper>

    <AwardWrapper animation="stars">
      <div class="award"></div>
    </AwardWrapper>
  </div>
</template>

<script setup lang="ts">
import AwardWrapper from "./components/AwardWrapper.vue";
</script>

<style>
html,
body,
#app {
  height: 100%;
}

.container {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 50px;
}

.award {
  width: 48px;
  height: 48px;
  background-color: gold;
  border-radius: 50%;
}
</style>
