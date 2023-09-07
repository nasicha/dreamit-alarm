<template>
  <Transition name="fade">
    <div v-if="!hideOverlay" class="overlay">
      <div class="overlay-wrapper" v-if="!hideOverlayWrapper && !countdownStarted">
        <div>
          <p>Dauer:</p>
          <input v-model="timer" type="number" />
        </div>
        <p>
          Alarm startet in <span>{{ timer }} Sekunden.</span>
        </p>
        <div>
          <p>Fade out:</p>
          <input v-model="hideTimerAfterSec" type="number" />
        </div>
        <p>
          Text verschwindet bei <span>{{ hideTimerAfterSec }} Sekunden.</span>
        </p>
        <button @click="startCountdown">Start</button>
      </div>
      <div class="overlay-wrapper bigTime" v-if="!hideOverlayWrapper && countdownStarted">
        <h1>{{ timer }}</h1>
      </div>
    </div>
  </Transition>
  <Transition name="fade-grow">
    <AlarmPage />
  </Transition>
</template>
<script setup lang="ts">
const timer = ref(7);
const hideTimerAfterSec = ref(3);
const countdownStarted = ref(false);
const hideOverlay = ref(false);
const hideOverlayWrapper = ref(false);

const startCountdown = () => {
  countdownStarted.value = true;
  const countdownInterval = setInterval(() => {
    if (timer.value > 0) {
      timer.value--;
    } else {
      clearInterval(countdownInterval);
    }
  }, 1000);
};

watch(timer, (value) => {
  if (value === 0) {
    hideOverlay.value = true;
  }
  if (value === hideTimerAfterSec.value) {
    hideOverlayWrapper.value = true;
  }
});

useHead({
  title: "DreamIt - Alarm",
  link: [
    {
      rel: "preload",
      as: "video",
      href: "../assets/video/gutenMorgen.webm",
      type: "video/webm",
    },
  ],
});
</script>
<style lang="scss">
@use '../assets/scss/transition.scss';

.overlay {
  position: absolute;
  height: 100vh;
  width: 100vw;
  background-color: black;
  z-index: 100;
  color: white;

  &-wrapper {
    display: flex;
    flex-direction: column;
    padding: 2rem;
    height: 80vh;

    &.bigTime {
      justify-content: center;
      align-items: center;
      & h1 {
        font-size: 5rem;
      }
    }

    & div {
      display: flex;
      align-items: center;
      justify-content: flex-start;
      gap: 1rem;
    }

    & button {
      height: 50px;
      background-color: black;
      color: white;
      border-color: white;
      margin-top: auto;
    }
  }
}
</style>
