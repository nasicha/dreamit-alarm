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
    video.value.play();
  }
});
</script>
<style lang="scss">
.morning-video {
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: cover;
  top: 0;
  z-index: 15;
  left: 0;
  right: 0;
  margin-left: auto;
  margin-right: auto;
}
</style>
