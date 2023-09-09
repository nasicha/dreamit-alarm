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
const timer = ref(0);
const hideTimerAfterSec = ref(0);
const countdownStarted = ref(false);
const hideOverlay = ref(false);
const hideOverlayWrapper = ref(false);

onMounted(() => {
  if (process.client) {
    const storedTimer = localStorage.getItem("timer");
    if (storedTimer === null) {
      timer.value = 5;
      localStorage.setItem("timer", timer.value.toString());
    } else {
      timer.value = parseInt(storedTimer);
    }
    const storedHideTimer = localStorage.getItem("hideTimer");
    if (storedHideTimer === null) {
      hideTimerAfterSec.value = 3;
      localStorage.setItem("hideTimer", hideTimerAfterSec.value.toString());
    } else {
      hideTimerAfterSec.value = parseInt(storedHideTimer);
    }
  }
});

const startCountdown = () => {
  countdownStarted.value = true;
  localStorage.setItem("timer", timer.value.toString());
  localStorage.setItem("hideTimer", hideTimerAfterSec.value.toString());
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
      href: "../assets/video/gutenMorgen.mp4",
      type: "video/mp4",
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
