<template>
  <div class="about">
    <h1>This is an about page</h1>

    <div>
    <div class='videoBox'>
      <YoutubeVue3 ref="youtube" :videoid="isDefault? playTime.video_id : playLive.video_id"
        style='width:100% ;height:600px'
        :autoplay="autoplay"
        @played="onPlayed"
        :loop="loop"/>
    </div>
    </div>

  </div>
</template>

<script>
import { YoutubeVue3 } from 'youtube-vue3'
var tag = document.createElement('script')
tag.src = 'https://www.youtube.com/iframe_api'
var firstScriptTag = document.getElementsByTagName('script')[0]
firstScriptTag.parentNode.insertBefore(tag, firstScriptTag)
export default {
  props: {
    autoplay: {
      type: Number,
      default: 1
    }
  },
  data () {
    return {
      setTimeHour: '',
      setTimeMins: '',
      loop: 1,
      playLive: {
        video_id: '2mCSYvcfhtc'
      },
      playTime: {
        video_id: 'pvYMUF-PMnc'
      },
      isDefault: false,
      arVideo: null,
      timer: 0
    }
  },
  components: {
    YoutubeVue3
  },
  methods: {
    getDuration () {
      this.$refs.youtube.player.getPlayerState().then((getPlayerState) => {
        console.log(getPlayerState)
      })
      this.$refs.youtube.player.getDuration().then((duration) => {
        console.log(duration)
      })
    },
    onPlayed () {
      console.log(this.$refs.youtube.player)
      // this.$refs.youtube.player.playVideo()
      this.getDuration()
      console.log('## OnPlayed')
    },
    changeTime () {
      const date = Date.now()
      const hours = new Date().getHours(date)
      const mins = new Date().getMinutes(date)

      // 轉成字串，如果低於10，前面加上'0'
      const hourString = (hours < 10) ? ('0' + hours) : ('' + hours)
      const minString = (mins < 10) ? ('0' + mins) : ('' + mins)

      this.setTimeHour = `${hourString}`
      this.setTimeMins = `${minString}`

      const timeZero = '00'

      if (this.setTimeHour >= 8 && this.setTimeHour <= 20) {
        if (this.setTimeMins === timeZero) {
          this.isDefault = true // 播放蜘蛛人
          // 取得蜘蛛人的影片總長度
          this.$refs.youtube.player.getDuration().then((duration) => {
            this.arVideo = duration
          })
          // 取得蜘蛛人影片當下的播放時間
          this.$refs.youtube.player.getCurrentTime().then((CurrentTime) => {
            this.timer = CurrentTime
          })
          console.log('1.蜘蛛人', this.arVideo) // 89.501 (影片總長度)
          console.log('getCurrentTime', this.timer) // 從 0 開始跑
        } else if (this.timer < this.arVideo) {
          this.$refs.youtube.player.getCurrentTime().then((CurrentTime) => {
            this.timer = CurrentTime
          })
          this.isDefault = true
          // this.$refs.youtube.player.playVideo()
          console.log('2.繼續播蜘蛛人 this.timer', this.timer)
          console.log('2.繼續播蜘蛛人 this.arVideo', this.arVideo)
        } else if (this.timer > this.arVideo) {
          console.log('3.timer > AR咕咕鐘', this.timer)
          this.isDefault = false
        } else {
          // this.$refs.youtube.player.playVideo()
          console.log('4.最後面else的this.timer', this.timer, this.timer === this.arVideo) // 播完AR咕咕鐘之後，this.timer的值會是89.501
          this.isDefault = false
        }
      }
    }
  },
  mounted () {
    this.onPlayed()
    setInterval(() => {
      this.changeTime()
    }, 1000)
  }
}
</script>

<style lang="scss" scoped>

</style>
