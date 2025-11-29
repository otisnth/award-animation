<template>
  <div class="container">
    <AwardWrapper animation="sparks">
      <AwardStub />
    </AwardWrapper>

    <AwardWrapper animation="stars">
      <AwardStub />
    </AwardWrapper>

    <AwardWrapper animation="confetti">
      <AwardStub />
    </AwardWrapper>

    <AwardWrapper animation="bubbles">
      <AwardStub />
    </AwardWrapper>

    <AwardWrapper animation="hearts">
      <AwardStub />
    </AwardWrapper>

    <AwardWrapper animation="fireworks">
      <AwardStub />
    </AwardWrapper>

    <AwardWrapper animation="flames">
      <AwardStub />
    </AwardWrapper>

    <AwardWrapper animation="rainbow">
      <AwardStub />
    </AwardWrapper>

    <AwardWrapper animation="holographic">
      <AwardStub />
    </AwardWrapper>

    <AwardWrapper animation="fog">
      <AwardStub />
    </AwardWrapper>

    <AwardWrapper animation="frozen">
      <AwardStub />
    </AwardWrapper>
  </div>
</template>

<script setup lang="ts">
import AwardWrapper from "./components/AwardWrapper.vue";
import AwardStub from "./components/AwardStub.vue";
// пришивание
// разрезание ножницами?
// рост травы или цветочка - делать
// замерзание и откалывание кусками в приоритете
// прилетает тарелка и сбрасывает медаль в приоритете
</script>

<style>
html,
body,
#app {
  min-height: 100vh;
  background-color: #e7e7e7;
}

.container {
  height: 100%;
  display: grid;
  justify-content: center;
  align-items: center;
  /* flex-wrap: wrap; */
  gap: 100px;
}
</style>
