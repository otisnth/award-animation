<template>
  <div>
    <div class="flame"></div>
    <div class="content">
      <slot />
    </div>
  </div>
</template>

<style>
.flame {
  width: 100%;
  height: 100%;
  background: linear-gradient(-45deg, red, orange);
  border-radius: 50px 50px 0px 50px;
  transform: rotate(-135deg);
  animation: 1s flame infinite;
  filter: blur(5px);
  position: absolute;
  box-shadow: 17px 20px 90px #700;
  border: 25px solid black;
  border-left-width: 25px;
  border-top-width: 25px;
  top: -30%;
  left: -50%;

  &:after,
  &:before {
    content: "";
    width: 30px;
    height: 30px;
    display: block;
    position: absolute;
    background: linear-gradient(-45deg, red, orange);
    animation: 2s flame infinite;
    transform: scale(0.8) rotate(20deg);
    border-radius: 30px 30px 0px 30px;
    top: 20px;
  }
  &:before {
    top: 0;
    animation-duration: 1s;
    transform: scale(0.9) rotate(-15deg) translate(10px, 0px);
  }
}

.content {
  position: relative;
  z-index: 2;
}

@keyframes flame {
  0% {
    height: 100%;
    width: 100%;
  }
  50% {
    height: 80%;
    width: 80%;
  }
  100% {
    height: 100%;
    width: 100%;
  }
}
</style>
