<template>
  <div class="comment_list" ref="dlSong">
    <div>
      <dl v-for="(item, index) in songCom" :key="index">
        <dt>
          <img :src="item.user.avatarUrl" alt />
        </dt>
        <dd class="name">{{item.user.nickname}}</dd>
        <dd class="detail">{{item.content}}</dd>
      </dl>
    </div>
  </div>
</template>

<script>
import IScroll from 'iscroll'
export default {
    data() {
        return {
            myScroll:'',
            songCom:[]
        }
    },
    mounted() {
        this.myScroll = new IScroll(this.$refs.dlSong,{
            mouseWheel:true,
            scrollbars:true
        })
    },
    methods: {
        getmyScroll(){
            this.$nextTick(()=>{
                this.myScroll.refresh();
            })
        }
    },
};
</script>

<style>
</style>