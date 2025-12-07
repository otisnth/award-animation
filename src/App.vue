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

    <AwardWrapper animation="ufo">
      <AwardStub />
    </AwardWrapper>

    <AwardWrapper animation="divine">
      <AwardStub />
    </AwardWrapper>

    <AwardWrapper animation="shimmer">
      <AwardStub />
    </AwardWrapper>

    <AwardWrapper animation="pulse">
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
// бутон раскрывается в медаль, и крутится цветок
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
  grid-template-columns: repeat(3, 1fr);
  justify-items: center;
  align-items: center;
  gap: 100px;

  padding: 100px;
}
</style>
