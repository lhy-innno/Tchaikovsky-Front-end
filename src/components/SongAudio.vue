<template>
  <div class="song-audio">
    <audio id="audio" :src="url" controls="controls" ref="player" preload="true" @canplay="startPlay" @ended="ended" @timeupdate="timeUpdate" >
      <!--（1）属性：controls，preload（2）事件：canplay，timeupdate，ended（3）方法：play()，pause() -->
      <!--controls：向用户显示音频控件（播放/暂停/进度条/音量）-->
      <!--preload：属性规定是否在页面加载后载入音频-->
      <!--canplay：当音频/视频处于加载过程中时，会发生的事件-->
      <!--timeupdate：当目前的播放位置已更改时-->
      <!--ended：当目前的播放列表已结束时-->
    </audio>
  </div>
</template>

<script>
import {mapGetters} from 'vuex'
export default {
  name: 'song-audio',
  computed: {
    ...mapGetters([
      'id', // 音乐id
      'url', // 音乐链接
      'listOfSongs', // 存放的音乐
      'isPlay', // 播放状态
      'volume', // 音量
      'curTime', // 当前音乐的播放位置
      'changeTime', // 指定播放时刻
      'autoNext' // 用于触发自动播放下一首
    ])
  },
  watch: {
    isPlay () {
      this.togglePlay()
    },
    changeTime () {
      console.log(this.changeTime)
      let player = this.$refs.player
      player.currentTime = this.changeTime
      console.log(player.currentTime)
      // this.$store.commit('setCurTime', this.changeTime)
    },
    volume (val) {
      this.$refs.player.volume = val
    }
  },
  methods: {
    togglePlay () {
      let player = this.$refs.player
      if (this.isPlay) {
        player.play()
      } else {
        player.pause()
      }
    },
    startPlay () {
      let player = this.$refs.player
      this.$store.commit('setDuration', player.duration)
      //  开始播放
      player.play()
      this.$store.commit('setIsPlay', true)
    },
    timeUpdate () {
      let player = this.$refs.player
      this.$store.commit('setCurTime', player.currentTime)
    },
    ended () {
      this.$store.commit('setIsPlay', false)
      this.$store.commit('setAutoNext', !this.autoNext)
      this.$store.commit('setCurTime', 0)
    }
  }
}
</script>

<style lang="scss" scoped>
  @import "src/assets/css/song-audio.scss";
</style>
