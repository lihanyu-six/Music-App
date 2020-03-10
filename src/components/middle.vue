<template>
  <div class="player_con">
    <img src="../assets/images/player_bar.png" class="play_bar" :class ="{playing}" />
    <!-- 黑胶碟片 -->
    <img src="../assets/images/disc.png" class="disc autoRotate" :class ="{playing}" />
    <img src="../assets/./images/cover.png" class="cover autoRotate" :class ="{playing}" />
  </div> 
</template>

<script>
export default {
    data() {
        return {
            playing:false
        }
    },
};
</script>

<style>
</style>