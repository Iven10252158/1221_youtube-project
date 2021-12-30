<template>
  <div class="about">
    <h1>This is an about page</h1>

    <div>
    <div class='videoBox'>
      <YoutubeVue3 ref="youtube" :videoid="playNow"
        style='width:100% ;height:600px'
        :autoplay="autoplay"
        @played="onPlayed"/>
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
    },
    loop: {
      type: Number,
      default: 1
    },
    videoid: {
      type: String
    }
  },
  data () {
    return {
      setTimeHour: '',
      setTimeMins: '',
      loopPlaylists: true,
      video1: {
        video_id: 'T1DXPL2t64k'
      },
      video2: {
        video_id: 'pvYMUF-PMnc'
      },
      video3: {
        video_id: 'mF3bYa4U6yA'
      },
      playNow: '',
      theseTime: '',
      isDefault: false,
      arVideo: null,
      timer: ''
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
      // this.$refs.youtube.player.getDuration().then((duration) => {
      //   console.log(duration)
      // })
    },
    onPlayed () {
      console.log(this.$refs.youtube.player)
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
      this.theseTime = `${hourString}${minString}`
      if (this.setTimeHour >= 10 && this.setTimeHour <= 21) {
        this.$refs.youtube.player.getPlayerState().then((getPlayerState) => {
          this.timer = getPlayerState
          if (this.timer !== 0) {
            console.log(this.timer)
            this.timer = 1
            console.log(this.timer)
          } else if (this.timer === 1) {
            console.log(this.timer)
            this.timer = 1
            this.playNow = this.video2.video_id
            console.log(this.timer)
          }
        })
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
