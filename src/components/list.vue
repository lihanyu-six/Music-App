<template>
  <div class="song_wrapper" ref="song_wrapper">
    <ul class="song_list">
      <li v-for="(item, index) in songList" :key="index" @click="liClick(item.id)">{{item.name}}</li>
    </ul>
  </div>
</template>

<script>
import IScroll from 'iscroll'
export default {
    data() {
        return {
            mySroll:'',
            songList:[]
        }
    },
    mounted() {
        this.mySroll = new IScroll(this.$refs.song_wrapper,{
            mouseWheel:true,
            scrollbars:true
        })
    },
    methods: {
        mySrollfresh(){
            this.$nextTick(()=>{
               this.mySroll.refresh(); 
            })
        },
        liClick(id){
            this.$parent.getSongUrl(id);
        }
    },
};
</script>

<style>
</style>