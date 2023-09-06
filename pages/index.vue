<template>
  <Transition name="fade">
    <div v-if="!hideOverlay" class="overlay">
      <div class="overlay-wrapper" v-if="!hideOverlayWrapper" min="8">
        <span>Set timer:</span>
        <input v-model="timer" type="number" />
        <span>{{ timer }}</span>
        <button @click="startCountdown">Start</button>
      </div>
    </div>
  </Transition>
  <Transition name="fade">
    <AlarmPage />
  </Transition>
</template>
<script setup lang="ts">


const timer = ref(10);
const hideOverlay = ref(false);
const hideOverlayWrapper = ref(false);

const startCountdown = () => {
  const countdownInterval = setInterval(() => {
    if (timer.value > 0) {
      timer.value--;
    } else {
      clearInterval(countdownInterval);
    }
  }, 1000);
}

watch(timer, (value) => {
  if (value === 0) {
    hideOverlay.value = true;
  }
  if(value === 5) {
    hideOverlayWrapper.value = true;
  }
})
</script>
<style lang="scss">
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
  }
}
</style>