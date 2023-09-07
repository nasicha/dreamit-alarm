<template>
  <div class="page">
    <Background />
    <div class="top">
      <SmartphoneTopbar />
    </div>
    <Transition name="fade">
      <AlarmInfo />
    </Transition>
    <div class="bottom"></div>
  </div>
</template>

<script setup>
import SmartphoneTopbar from "@/assets/img/smartphone-topbar.svg?component";
import { useVibrate } from "@vueuse/core";

const { vibrate, stop, isSupported } = useVibrate({ pattern: [300, 100, 300] });

const vibrating = ref(false);

const vibrateDevice = () => {
  if (isSupported.value) {
    vibrating.value = !vibrating.value;
    vibrating ? vibrate() : stop();
  }
};
const vibrateNow = () => {
  window.navigator.vibrate([
    200,
    100,
    200,
    200,
    100,
    200,
    200,
    100,
    200,
    200,
    100,
    200,
    200,
    100,
    200,
    200,
    100,
    200,
    200,
    100,
    200,
    200,
    100,
    200,
    200,
    100,
    200,
    200,
    100,
    200,
  ]);
};
</script>
<style lang="scss">
@use '../assets/scss/transition.scss';

.page {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

.top {
  height: 5vh;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  z-index: 20;
  position: relative;

  svg {
    padding: 1rem;
  }
}

.bottom {
  height: 12vh;
}

.word {
  letter-spacing: 0.5px;
}
</style>
