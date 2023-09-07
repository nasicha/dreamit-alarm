<template>
  <div class="mid" v-if="showAlarm" >
      <img src="~assets/img/WordAlarm.png" class="word" />
    <div class="mid-alarm">
      <img src="~assets/img/GLOW.png" class="glow" />
      <img src="~assets/img/clock-alarm.png" class="clock" />

    </div>
    <div ref="deactivateAlarm">
      <DeactivateAlarm id="deactivateAlarmBtn" />
    </div>
  </div>
  <div v-if="!showAlarm">
    <video src="~assets/video/gutenMorgen.webm" autoplay muted type="video/webm">
        <p>Your browser does not support the video tag.</p>
    </video>
  </div>
</template>
<script setup lang="ts">
import DeactivateAlarm from "@/assets/img/deactivate-alarm.svg?component";
import { useSwipe } from '@vueuse/core'
import gsap from 'gsap'


const showAlarm = ref(true);

const toggleAlarm = () => {
  showAlarm.value = !showAlarm.value;
}

const deactivateAlarm = ref(null)
const { direction } = useSwipe(
  deactivateAlarm, { // your ref element 
        onSwipe() {
          if(direction.value === 'up') {

            gsap.to("#deactivateAlarmBtn", {
              duration: .55,
              y: -100,
            });
            
            setTimeout(() => {
              toggleAlarm()
            }, 500);
          }
        },
    }
)



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
  max-width: 80px;;
}

@keyframes pulse {
	0% {
		transform: scale(0.90);
	}

	50% {
		transform: scale(1);
	}

	100% {
		transform: scale(0.90);
	}
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>