<template>
  <video class="morning-video" ref="video">
    <source src="~assets/video/gutenMorgen.mp4" type="video/mp4" />
    <p>Your browser does not support the video tag.</p>
  </video>
</template>
<script setup lang="ts">
const video = ref(null);
onMounted(() => {
  if (video.value) {
    console.log(typeof video.value);
    video.value.play();
  }
});
</script>
<style lang="scss">
.morning-video {
  max-width: 100%;
  margin-top: -5vh;
  z-index: 15;
}
</style>
