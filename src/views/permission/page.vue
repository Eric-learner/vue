<!--<template>-->
<!--  <div class="liveView">-->
<!--    <video-player-->
<!--      ref="videoPlayer"-->
<!--      class="vjs-custom-skin"-->
<!--      :options="playerOptions"-->
<!--      :playsinline="playsinline"-->
<!--      @ready="onPlayerReadied"-->
<!--      @timeupdate="onTimeupdate"-->
<!--    />-->
<!--  </div>-->
<!--</template>-->

<!--<script>-->
<!--import { videoPlayer } from 'vue-video-player'-->
<!--import 'video.js/dist/video-js.css'-->
<!--import 'vue-video-player/src/custom-theme.css'-->
<!--import 'videojs-contrib-hls'-->

<!--export default {-->
<!--  name: 'Live',-->
<!--  components: {-->
<!--    videoPlayer-->
<!--  },-->
<!--  data() {-->
<!--    return {-->
<!--      initialized: false,-->
<!--      currentTech: '',-->
<!--      streams: {-->
<!--        rtmp: '',-->
<!--        hls: ''-->
<!--      },-->
<!--      playerOptions: {-->
<!--        overNative: true,-->
<!--        autoplay: false,-->
<!--        controls: true,-->
<!--        techOrder: ['html5'],-->
<!--        sourceOrder: true,-->
<!--        flash: {-->
<!--          hls: { withCredentials: false }-->
<!--        },-->
<!--        html5: { hls: { withCredentials: false }},-->
<!--        sources: [-->
<!--          {-->
<!--            withCredentials: false,-->
<!--            type: 'application/x-mpegURL',-->
<!--            src: 'http://hls01open.ys7.com/openlive/011d517d0de04b9fb0b483208c726e55.hd.m3u8'-->
<!--          }-->
<!--        ]-->
<!--      }-->
<!--    }-->
<!--  },-->
<!--  computed: {-->
<!--    player() {-->
<!--      return this.$refs.videoPlayer.player-->
<!--    },-->
<!--    currentStream() {-->
<!--      return this.currentTech === 'Flash' ? 'RTMP' : 'HLS'-->
<!--    },-->
<!--    playsinline() {-->
<!--      const ua = navigator.userAgent.toLocaleLowerCase()-->
<!--      // x5内核-->
<!--      if (ua.match(/tencenttraveler/) != null || ua.match(/qqbrowse/) != null) {-->
<!--        return false-->
<!--      } else {-->
<!--        // ios端-->
<!--        return true-->
<!--      }-->
<!--    }-->
<!--  },-->
<!--  methods: {-->
<!--    onPlayerReadied() {-->
<!--      if (!this.initialized) {-->
<!--        this.initialized = true-->
<!--        this.currentTech = this.player.techName_-->
<!--      }-->
<!--    },-->
<!--    // record current time-->
<!--    onTimeupdate(e) {-->
<!--      console.log('currentTime', e.cache_.currentTime)-->
<!--    }-->
<!--  }-->
<!--}-->
<!--</script>-->

<!--<style scoped>-->
<!--.liveView {-->
<!--  position: relative;-->
<!--}-->
<!--</style>-->

<template>
  <div class="player">
    <h3> 请输入视频流地址进行目标检测</h3>
    <div class="left">
      <button @click="switchPlayer1">Play1</button>
      <input v-model="src1" type="text" style="width: 500px">
    </div>
    <video-player
      ref="videoPlayer"
      class="vjs-custom-skin1"
      :options="playerOptions1"
      @play="onPlayerPlay($event)"
      @ready="onPlayerReady($event)"
    />
    <h3>处理后的视频流如下</h3>
    <div>
      <button @click="switchPlayer2">Play2</button>
      <input v-model="src2" type="text" style="width: 500px">
    </div>
    <video-player
      ref="videoPlayer2"
      class="vjs-custom-skin2"
      :options="playerOptions2"
      @play="onPlayerPlay2($event)"
      @ready="onPlayerReady2($event)"
    />
    <el-button icon="el-icon-question" type="primary" target="_blank" onclick="window.open('http://www.5guav.com.cn/') " >
      NIUVS团队提供技术支持
    </el-button>
  </div>

</template>
<script>
import VideoPlayer from '@/views/permission/components/VideoPlayer'
export default {
  name: 'Home',
  components: {
    VideoPlayer
  },
  data() {
    return {
      // src: 'https://bitdash-a.akamaihd.net/content/sintel/hls/playlist.m3u8', // 这个测试流也可以用！
      src1: 'https://bitdash-a.akamaihd.net/content/sintel/hls/playlist.m3u8', // 这个测试流也可以用！
      src2: 'https://bitdash-a.akamaihd.net/content/MI201109210084_1/m3u8s/f08e80da-bf1d-4e3d-8899-f0f6155f6efa.m3u8', // 这个测试流也可以用！
      // src1: '',
      // src2: '',
      playerOptions1: {
        autoplay: true,
        controls: true,
        controlBar: {
          timeDivider: false,
          durationDisplay: false
        }
      },
      playerOptions2: {
        autoplay: true,
        controls: true,
        controlBar: {
          timeDivider: false,
          durationDisplay: false
        }
        // poster: 'https://surmon-china.github.io/vue-quill-editor/static/images/surmon-5.jpg'
      }
    }
  },
  computed: {
    player() {
      return this.$refs.videoPlayer.player
    },
    player2() {
      return this.$refs.videoPlayer2.player
    }
  },
  mounted() {
    // const src1 =
    //   'https://bitdash-a.akamaihd.net/content/MI201109210084_1/m3u8s/f08e80da-bf1d-4e3d-8899-f0f6155f6efa.m3u8'
    // this.playVideo(src1)
    // const src2 =
    //   'https://bitdash-a.akamaihd.net/content/MI201109210084_1/m3u8s/f08e80da-bf1d-4e3d-8899-f0f6155f6efa.m3u8'
    // this.playVideo(src2)
  },
  methods: {
    onPlayerPlay(player) { console.log('1111') },
    onPlayerReady(player) {
      this.player.play()
      console.log('1111+1111')
    },
    onPlayerPlay2(player) { console.log('2222') },
    onPlayerReady2(player) {
      this.player2.play()
      console.log('2222+2222')
    },

    playVideo: function(source) {
      console.log(source)
      const video = {
        withCredentials: false,
        type: 'application/x-mpegurl',
        src: source
      }
      this.player.reset() // in IE11 (mode IE10) direct usage of src() when <src> is already set, generated errors,
      this.player.src(video)
      // this.player.src1()
      // this.player.src2(video)
      // this.player.load()
      this.player.play()
    },
    playVideo2: function(source) {
      console.log(source)
      const video = {
        withCredentials: false,
        type: 'application/x-mpegurl',
        src: source
      }
      this.player2.reset() // in IE11 (mode IE10) direct usage of src() when <src> is already set, generated errors,
      this.player2.src(video)
      // this.player.src1()
      // this.player.src2(video)
      // this.player.load()
      this.player2.play()
    },
    switchPlayer1: function() {
      this.playVideo(this.src1)
      console.log(this.src1)
    },
    switchPlayer2: function() {
      this.playVideo2(this.src2)
      console.log(this.src2)
      console.log(this.HTTP_URL)
    }
  }
}
</script>
<style scoped>
.player {
  position: absolute !important;
  width: 20%;
  height: 30%;
  /*display: flex;*/
  /*flex-direction: row;*/
  /*justify-content:space-around ;*/
  /*align-items:flex-start;*/
  /*flex-wrap:wrap-reverse;*/
}
.vjs-custom-skin1 {
  height: 90% !important;
  flex:1;
}
.vjs-custom-skin1 /deep/ .video-js {
  width: 100% !important;
  height: 90%;
  /*flex:1;*/
}

.vjs-custom-skin2 {

  height: 90% !important;
  /*flex:1;*/
}
.vjs-custom-skin2 /deep/ .video-js {
  width: 100% !important;
  height: 90%;
  /*flex:1;*/
}
</style>
