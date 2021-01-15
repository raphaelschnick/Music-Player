<template>
  <div id="app">
    <div class="player">
      <div class="cover">
        <img :src="current.cover" />
      </div>
      <navbar></navbar>
      <div class="player-ui">
        <div class="title">
          <h3>{{ current.title }}</h3>
        </div>
        <div class="small">
          <i class="material-icons">replay</i>
          <p>{{ current.artist }}</p>
          <i class="material-icons">volume_up</i>
          {{ duration }}
        </div>
        <div class="progress">
          <div class="played">
            <div class="circle"></div>
          </div>
        </div>
        <div class="controls">
          <i class="material-icons" @click="prev">skip_previous</i>
          <!-- eslint-disable-next-line prettier/prettier !-->
          <i class="material-icons" v-if="!isPlaying" @click="play">play_arrow</i>
          <i class="material-icons" v-else @click="pause">pause</i>
          <i class="material-icons" @click="next">skip_next</i>
        </div>
      </div>
      <div class="btn">
        <i class="material-icons" @click="shuffle">shuffle</i>
      </div>
      <div class="music">
        <div v-for="song in songs" :key="song.src" @click="play(song)">
          <div class="song">
            <div class="info">
              <div class="img">
                <img :src="song.cover" />
              </div>
              <div class="titles">
                <h5>{{ song.title }}</h5>
                <p>{{ song.artist }}</p>
              </div>
            </div>
            <div v-if="current == song" class="state playing">
              <i class="material-icons">equalizer</i>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from '../components/Navbar.vue'
export default {
  name: 'player',
  components: {
    Navbar
  },
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      duration: null,
      currentTime: null,
      isTimerPlaying: false,
      songs: [
        {
          title: 'Anthrazit',
          artist: 'RAF Camora',
          src: require('../assets/anthrazit.mp3'),
          cover: require('../assets/anthrazit.jpg'),
          favorited: false
        },
        {
          title: 'Erober die Welt',
          artist: 'Ufo361',
          src: require('../assets/erober_die_welt.mp3'),
          cover: require('../assets/erober_die_welt.jpg'),
          favorited: false
        },
        {
          title: 'Sommertime Sadness',
          artist: 'Lana Del Rey',
          src: require('../assets/summertime_sadness.mp3'),
          cover: require('../assets/summertime_sadness.jpg'),
          favorited: false
        },
        {
          title: 'Alles Probiert',
          artist: 'RAF Camora',
          src: require('../assets/alles_probiert.mp3'),
          cover: require('../assets/anthrazit.jpg'),
          favorited: false
        },
        {
          title: 'Vienna',
          artist: 'RAF Camora',
          src: require('../assets/vienna.mp3'),
          cover: require('../assets/anthrazit_rr.jpg'),
          favorited: false
        },
        {
          title: 'Andere Liga',
          artist: 'RAF Camora',
          src: require('../assets/andere_liga.mp3'),
          cover: require('../assets/anthrazit.jpg'),
          favorited: false
        },
        {
          title: 'Was jetzt',
          artist: 'RAF Camora',
          src: require('../assets/was_jetzt.mp3'),
          cover: require('../assets/anthrazit_rr.jpg'),
          favorited: false
        },
        {
          title: 'Augenblick',
          artist: 'RAF Camora',
          src: require('../assets/augenblick.mp3'),
          cover: require('../assets/anthrazit.jpg'),
          favorited: false
        },
        {
          title: 'Luft',
          artist: 'RAF Camora',
          src: require('../assets/luft.mp3'),
          cover: require('../assets/anthrazit.jpg'),
          favorited: false
        },
        {
          title: 'Kontrollieren',
          artist: 'RAF Camora',
          src: require('../assets/kontrollieren.mp3'),
          cover: require('../assets/anthrazit.jpg'),
          favorited: false
        },
        {
          title: 'Teflon',
          artist: 'RAF Camora',
          src: require('../assets/teflon.mp3'),
          cover: require('../assets/anthrazit.jpg'),
          favorited: false
        },
        {
          title: 'Roots',
          artist: 'RAF Camora',
          src: require('../assets/roots.mp3'),
          cover: require('../assets/anthrazit.jpg'),
          favorited: false
        },
        {
          title: 'Donna Imma',
          artist: 'RAF Camora',
          src: require('../assets/donna_imma.mp3'),
          cover: require('../assets/anthrazit.jpg'),
          favorited: false
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play(song) {
      if (typeof song.src != 'undefined') {
        this.current = song
        this.player.src = this.current.src
      }

      this.player.play()
      this.isTimerPlaying = true
      this.player.addEventListener(
        'ended',
        function() {
          this.index++
          if (this.index > this.songs.length - 1) {
            this.index = 0
          }

          this.current = this.songs[this.index]
          this.play(this.current)
        }.bind(this)
      )
      this.isPlaying = true
    },
    shuffle(song) {
      this.play(this.player.shuffle)
    },
    pause() {
      this.player.pause()
      this.isPlaying = false
      this.isTimerPlaying = false
    },
    next() {
      this.index++
      if (this.index > this.songs.length - 1) {
        this.index = 0
      }

      this.current = this.songs[this.index]
      this.play(this.current)
    },
    prev() {
      this.index--
      if (this.index < 0) {
        this.index = this.songs.length - 1
      }

      this.current = this.songs[this.index]
      this.play(this.current)
    }
  },
  created() {
    this.current = this.songs[this.index]
    this.player.src = this.current.src
  },
  generateTime() {
    let width = (100 / this.audio.duration) * this.audio.currentTime
    this.barWidth = width + '%'
    this.circleLeft = width + '%'
    let durmin = Math.floor(this.audio.duration / 60)
    let dursec = Math.floor(this.audio.duration - durmin * 60)
    let curmin = Math.floor(this.audio.currentTime / 60)
    let cursec = Math.floor(this.audio.currentTime - curmin * 60)
    if (durmin < 10) {
      durmin = '0' + durmin
    }
    if (dursec < 10) {
      dursec = '0' + dursec
    }
    if (curmin < 10) {
      curmin = '0' + curmin
    }
    if (cursec < 10) {
      cursec = '0' + cursec
    }
    this.duration = durmin + ':' + dursec
    this.currentTime = curmin + ':' + cursec
  },
  updateBar(x) {
    let progress = this.$refs.progress
    let maxduration = this.audio.duration
    let position = x - progress.offsetLeft
    let percentage = (100 * position) / progress.offsetWidth
    if (percentage > 100) {
      percentage = 100
    }
    if (percentage < 0) {
      percentage = 0
    }
    this.barWidth = percentage + '%'
    this.circleLeft = percentage + '%'
    this.audio.currentTime = (maxduration * percentage) / 100
    this.audio.play()
  },
  clickProgress(e) {
    this.isTimerPlaying = true
    this.audio.pause()
    this.updateBar(e.pageX)
  },
  prevTrack() {
    this.transitionName = 'scale-in'
    this.isShowCover = false
    if (this.currentTrackIndex > 0) {
      this.currentTrackIndex--
    } else {
      this.currentTrackIndex = this.tracks.length - 1
    }
    this.currentTrack = this.tracks[this.currentTrackIndex]
    this.resetPlayer()
  },
  nextTrack() {
    this.transitionName = 'scale-out'
    this.isShowCover = false
    if (this.currentTrackIndex < this.tracks.length - 1) {
      this.currentTrackIndex++
    } else {
      this.currentTrackIndex = 0
    }
    this.currentTrack = this.tracks[this.currentTrackIndex]
    this.resetPlayer()
  },
  resetPlayer() {
    this.barWidth = 0
    this.circleLeft = 0
    this.audio.currentTime = 0
    this.audio.src = this.currentTrack.source
    setTimeout(() => {
      if (this.isTimerPlaying) {
        this.audio.play()
      } else {
        this.audio.pause()
      }
    }, 300)
  }
}
</script>

<style>
body {
  color: #333;
  font-family: 'Roboto', sans-serif;
  font-size: 1em;
  -webkit-font-smoothing: antialiased;
  line-height: 1em;
}
* {
  box-sizing: border-box;
  user-select: none;
}
i {
  cursor: pointer;
}

.player .cover {
  z-index: -1;
  position: absolute;
  width: 100%;
  height: 432px;
  background: linear-gradient(
    -45deg,
    rgba(255, 0, 0, 0.3),
    rgba(0, 162, 255, 0.4)
  );
  background-size: 400%, 400%;
  animation: gradient 15s ease infinite;
  background-repeat: no-repeat;
}
@keyframes gradient {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}
.player .cover img {
  display: block;
  height: 50%;
  width: auto;
  margin-left: auto;
  margin-right: auto;
  margin-top: 50px;
}
.player .player-ui {
  z-index: 3;
  margin-top: 231px;
}
.player .player-ui .title {
  display: flex;
  justify-content: center;
  padding-top: 11px;
}
.player .player-ui .title h3 {
  margin: 0;
  color: black;
  font-size: 30px;
  font-weight: 100;
}
.player .player-ui .small {
  padding: 15px 35px 0px 35px;
  display: flex;
  justify-content: space-between;
}
.player .player-ui .small p,
.player .player-ui .small i {
  margin: 0;
  color: black;
  font-size: 14px;
}
.player .player-ui .small i {
  font-size: 16px;
}
.player .player-ui .progress {
  position: relative;
  height: 2px;
  margin: 30px 20px 0px 20px;
  background: rgba(255, 255, 255, 0.3);
}
.player .player-ui .progress .played {
  width: 20%;
  height: 2px;
  position: absolute;
  background: black;
}
.player .player-ui .progress .played .circle {
  width: 10px;
  height: 10px;
  background: black;
  border-radius: 50%;
  margin-left: 59px;
  margin-top: -4px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.7);
}
.player .player-ui .controls {
  display: flex;
  justify-content: space-between;
  padding: 25px 50px 0px 50px;
}
.player .player-ui .controls i {
  color: black;
  font-size: 32px;
}
.player .btn {
  margin-left: auto;
  margin-right: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  z-index: 4;
  margin-top: 20px;
  width: 40px;
  height: 40px;
  border-radius: 50%;
}
.player .btn i {
  margin-top: -51px;
  font-size: 28px;
  color: white;
}
.player .music {
  padding: 10px 20px 0px 20px;
  background: #1a1b1e;
  margin-top: -45px;
}
.player .music .song {
  cursor: pointer;
}
.player .music .song {
  height: 80px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
}
.player .music .song .info {
  display: flex;
  align-items: center;
}
.player .music .song .info .img {
  width: 60px;
  height: 60px;
  background: red;
  margin-right: 10px;
}
.player .music .song .info .img img {
  height: 60px;
  background-size: cover;
}
.player .music .song .info .titles h5,
.player .music .song .info .titles p {
  margin: 0;
  color: white;
}
.player .music .song .info .titles h5 {
  font-size: 17px;
  font-weight: 400;
  margin-bottom: 7px;
}
.player .music .song .info .titles p {
  font-size: 13px;
  color: #929292;
}
.player .music .song .state.playing i {
  color: #f44336;
}
.player .music .song .state i {
  color: #ddd;
}
</style>
