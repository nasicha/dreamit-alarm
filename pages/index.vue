<template>
  <div class="page">
    <Background />
    <div class="top">
      <SmartphoneTopbar />
    </div>
    <div class="mid">
      <DeactivateAlarm @click="vibrateDevice()" />
    </div>
    <div class="bottom">
    </div>
  </div>
</template>

<script setup>
import DeactivateAlarm from "@/assets/img/deactivate-alarm.svg?component";
import SmartphoneTopbar from "@/assets/img/smartphone-topbar.svg?component";
import { useVibrate } from '@vueuse/core'

const { vibrate, stop, isSupported } = useVibrate({ pattern: [300, 100, 300] })

const vibrating = ref(false);

const vibrateDevice = () => {
  if (isSupported.value) {
    vibrating.value = !vibrating.value;
    vibrating ? vibrate() : stop();
  }
}


useHead({
  title: 'DreamIt - Alarm',
})
</script>
<style lang="scss">
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

  svg {
    padding: 1rem;
  }
}

.mid {
  height: 83vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;

  &-top {
    height: 70vh;
  }
}

.bottom {
  height: 12vh;
}
</style>