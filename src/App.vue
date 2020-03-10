<template>
  <div class="wrap">
    <div class="play_wrap" id="player">
      <div class="search_bar">
        <img src="./assets/images/player_title.png" alt />
        <input type="text" @keyup.13="search" v-model="iptVal" />
      </div>
      <div class="center_con">
        <!-- 歌曲列表 -->
        <list ref="list"></list>
        <!-- 歌曲中间动画部分 -->
        <middle ref="middle"></middle>
        <!-- 评论模块 -->
        <comment ref="comment"></comment>
      </div>
      <div class="audio_con">
        <audio :src="songUrl" @play="songPlay" @pause="songPause" controls autoplay loop class="myaudio"></audio>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import list from "./components/list";
import middle from './components/middle'
import comment from './components/comment'
export default {
  components: {
    list,
    middle,
    comment
  },
  data() {
    return {
      songUrl:'',
      iptVal: "陈奕迅"
    };
  },
  methods: {
    search() {
      axios({
        url: "https://autumnfish.cn/search",
        params: {
          keywords: this.iptVal,
          xxx: Math.random() * 999
        }
      }).then(res => {
        // window.console.log(res)
        this.$refs.list.songList = res.data.result.songs;
        this.$refs.list.mySrollfresh();
        // window.console.log(this.$refs.list.songList)
      });
    },
    getSongUrl(id) {
      // 通过id获取url
      axios({
        url: "https://autumnfish.cn/song/url",
        params: {
          id: id,
          xxx: Math.random() * 999
        }
      }).then(res => {
        // window.console.log(res);
        this.songUrl = res.data.data[0].url;
      });
      // 通过id获取评论
      axios({
        url:'https://autumnfish.cn/comment/music',
        params:{
          id:id,
          xxx: Math.random() * 999
        }
      }).then(res=>{
        // window.console.log(res)
        this.$refs.comment.songCom = res.data.comments;
        this.$refs.comment.getmyScroll();
        // window.console.log(this.$refs.comment.songCom);
      })
    },
    songPlay(){
      this.$refs.middle.playing = true
    },
    songPause(){
      this.$refs.middle.playing = false
    }
  },
  created() {
    this.search()
  },
};
</script>

<style>
body,
ul,
dl,
dd {
  margin: 0px;
  padding: 0px;
}
.wrap {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: url("./assets/images/bagimg.jpg") no-repeat;
  background-size: 100% 100%;
}

.play_wrap {
  width: 800px;
  height: 544px;
  position: fixed;
  left: 50%;
  top: 50%;
  margin-left: -400px;
  margin-top: -272px;
  background-color: goldenrod;
  opacity: 0.9;
}

.search_bar {
  height: 60px;
  background-color: goldenrod;
  overflow: hidden;
  border-top-left-radius: 4px;
  border-top-right-radius: 4px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: relative;
  z-index: 11;
}

.search_bar img {
  margin-left: 23px;
}

.search_bar input {
  margin-right: 23px;
  width: 296px;
  height: 34px;
  border-radius: 17px;
  border: 0px;
  background: url("./assets/images/zoom.png") 265px center no-repeat
    rgba(255, 255, 255, 0.45);
  text-indent: 15px;
  outline: none;
}

.center_con {
  height: 435px;
  background-color: rgba(255, 255, 255, 0.5);
  display: flex;
  opacity: 0.9;
}

.song_wrapper {
  width: 200px;
  height: 435px;
  box-sizing: border-box;
  padding: 10px;
  list-style: none;
  background: url("./assets/images/line.png") right center no-repeat;
  position: relative;
  overflow: hidden;
}

.song_list li {
  font-size: 12px;
  color: #333;
  line-height: 36px;
  width: 180px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  cursor: pointer;
}

.song_list .active {
  color: #da651e;
}

.player_con {
  width: 400px;
  height: 435px;
  position: relative;
}

.disc {
  position: absolute;
  left: 73px;
  top: 60px;
  z-index: 9;
}

.cover {
  position: absolute;
  left: 125px;
  top: 112px;
  width: 150px;
  height: 150px;
  border-radius: 75px;
  z-index: 8;
}

.comment_list {
  width: 200px;
  height: 435px;
  box-sizing: border-box;
  padding: 10px;
  list-style: none;
  background: url("./assets/images/line.png") left center no-repeat;
  overflow: hidden;
  position: relative;
}

.comment_list dl {
  padding-left: 55px;
  position: relative;
  margin-bottom: 20px;
}
.comment_list dl:last-child {
  padding-bottom: 30px;
}

.comment_list dt {
  position: absolute;
  left: 4px;
  top: 0px;
}

.comment_list dt img {
  width: 40px;
  height: 40px;
  border-radius: 20px;
}

.comment_list dd {
  font-size: 12px;
}

.comment_list .name {
  font-weight: bold;
  color: #333;
  margin-top: 5px;
}

.comment_list .detail {
  color: #666;
  margin-top: 5px;
  line-height: 18px;
}

.audio_con {
  height: 50px;
  background-color: #f1f3f4;
  border-bottom-left-radius: 4px;
  border-bottom-right-radius: 4px;
}

.myaudio {
  width: 800px;
  height: 40px;
  margin-top: 5px;
  outline: none;
  background-color: #f1f3f4;
}

/* 旋转的动画 */
@keyframes Rotate {
  from {
    transform: rotateZ(0);
  }

  to {
    transform: rotateZ(360deg);
  }
}

/* 旋转的类名 */
.autoRotate {
  animation-name: Rotate;
  animation-iteration-count: infinite;
  animation-play-state: paused;
  animation-timing-function: linear;
  animation-duration: 5s;
}

/* 是否正在播放 */
.playing {
  animation-play-state: running;
}

.play_bar {
  position: absolute;
  left: 200px;
  top: -10px;
  z-index: 10;
  transform: rotate(-25deg);
  transform-origin: 12px 12px;
  transition: 1s;
}

/* 播放杆 转回去 */
.play_bar.playing {
  transform: rotate(0);
}
</style>