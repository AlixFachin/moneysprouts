<template>
  <div class="logo-panel">
    <img
      src="@/assets/sprout.svg"
      id="sprout1"
      style="--order: 1"
      width="50px"
      height="50px"
      alt="moneysprouts logo"
    />
  </div>
</template>

<script>
export default {
  name: "MainAnimation",
};
</script>

<style>
:root {
  --back-color: rgba(255, 255, 255, 0.7);
}

div.logo-panel {
  height: 3em;
  /*  To align the sprouts at the bottom of the stuff */
  display: flex;
  justify-content: space-around;
  align-items: flex-end;
  margin-left: 1em;
}

#sprout1,
#sprout2,
#sprout3 {
  animation-name: leafanimation;
  animation-duration: 2s;
  animation-iteration-count: infinite;
  animation-timing-function: ease-in-out;
  transform-origin: 50% 80%;
  animation-direction: alternate-reverse;
  transform: rotate(-15deg);
  animation-delay: calc(var(--order) * 550ms);
}

@keyframes leafanimation {
  0% {
    transform: translate(0, 0) rotate(-15deg);
  }
  100% {
    transform: translate(0, 0) rotate(15deg);
  }
}
</style>
