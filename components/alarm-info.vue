<template>
  <div class="mid" v-if="showAlarm">
    <img src="~assets/img/WordAlarm.png" class="word" />
    <div class="mid-alarm">
      <img src="~assets/img/GLOW.png" class="glow" />
      <img src="~assets/img/clock-alarm.png" class="clock" />
    </div>
    <div ref="deactivateAlarm">
      <DeactivateAlarm id="deactivateAlarmBtn" />
    </div>
  </div>
  <Transition name="fade-long">
    <div v-if="!showAlarm">
      <GmVideo />
    </div>
  </Transition>
</template>
<script setup lang="ts">
import DeactivateAlarm from "@/assets/img/deactivate-alarm.svg?component";
import { useSwipe } from "@vueuse/core";
import gsap from "gsap";

const showAlarm = ref(true);
const duration = ref(0.3);

const toggleAlarm = () => {
  setTimeout(() => {
    showAlarm.value = !showAlarm.value;
  }, duration.value * 1000);
};

const deactivateAlarm = ref(null);
const swiped = ref(false);

const { direction } = useSwipe(deactivateAlarm, {
  onSwipe() {
    if (direction.value === "up" && !swiped.value) {
      swiped.value = true;
      gsap.to("#deactivateAlarmBtn", {
        duration: duration.value,
        y: -100,
      });

      toggleAlarm();
    }
  },
});
</script>
<style lang="scss">
.mid {
  height: 83vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;

  &-alarm {
    position: relative;
    width: 330px;
    height: 330px;
    display: flex;
    justify-content: center;
    margin-bottom: 30px;
  }
}

.clock {
  width: 320px;
  height: 320px;
  z-index: 10;
}

.glow {
  width: 430px;
  height: 430px;
  position: absolute;
  z-index: 5;
  top: 0;
  margin-top: -50px;
  transform: scale(1);
  animation: pulse 1.5s infinite;
}

.word {
  max-width: 80px;
}
</style>
