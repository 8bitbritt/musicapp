<template>
  <div id="app">
    <header>
      <h1>Now Playing</h1>
    </header>
    <main>
     <section class="playlist">
        <h3>Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>

      <section class="player">
      <img class="album" :src="current.album" :alt="current.alt" />
        <h2 class="title">{{ current.title }}</h2>
        <span class="author">By {{ current.artist }}</span>
        <div class="controls">
          <button class="prev" @click="prev"><img src="./assets/prev.png" alt="previous" width="30px"></button>
          <button class="play" v-if="!isPlaying" @click="play"><img src="./assets/play.png" alt="play"></button>
          <button class="pause" v-else @click="pause"><img src="./assets/pause.png" alt="pause"></button>
          <button class="next" @click="next"><img src="./assets/next.png" alt="next" width="30px"></button>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'My Pal',
          artist: 'Crowander',
          album: require('./assets/album1.jpg'),
          alt: 'spacey album art',
          src: require('./assets/crowander-my-pal.mp3')
        },
        {
          title: 'Your Eyes',
          artist: 'Crowander',
          album: require('./assets/album2.jpg'),
           alt: 'spacey album art',
          src: require('./assets/crowander-your-eyes.mp3')
        },
        {
          title: 'Pillows',
          artist: 'Ketsa',
         album: require('./assets/album3.jpg'),
           alt: 'desert album art',
          src: require('./assets/ketsa-pillows.mp3')
        },
        {
          title: 'Get Suzie With The Corks',
          artist: 'Origami Repetika',
          album: require('./assets/album4.jpg'),
           alt: 'iceberg album art',
          src: require('./assets/origami-repetika-get-suzie-with-the-corks.mp3')
        },
          {
          title: 'Snow-Dawn',
          artist: 'Ketsa',
         album: require('./assets/album5.jpg'),
           alt: 'sky album art',
          src: require('./assets/ketsa-snow-dawn.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play (song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }//if the playlist is at an end start at the beginning
        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      this.isPlaying = true;
    },
    pause () {
      this.player.pause();
      this.isPlaying = false;
    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev () {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
}
</script>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
body {
  width:100%;
  height:100%;
	font-family: 'Source Sans Pro', sans-serif;
  font-size: 18px;
  line-height: 23px;
  background: rgb(58,77,137);
  background: linear-gradient(90deg, rgba(58,77,137,1) 0%, rgba(152,58,58,1) 100%);
}
img{
    min-width: 100%;
  max-width: 100%;
}

button{
  border:none;
}

h3{
  color: #535c69;
  padding-bottom:.5em;
  font-size: 1.5em;
}

h1{
  text-align: center;
  margin: 1em;
  color: #FFF;
  font-size: 2em;
}

main{
  display:flex;
  align-items:center;
  justify-content:space-between;
  max-width: 900px;
  margin:0 auto;
  padding: 25px;
}

.playlist{
  max-width: 500px;
  padding: 1.5em;
  background-color: #e2ebf1;
  border-radius: 25px;
  margin-right:20px;
}

.playlist button{
  display:block;
  background-color: transparent;
  border:none;
  text-align:left;
  padding: .4em 0;
  margin:0;
  font-size: 16px;
}

.playlist button.playing{
  color: #404C84;
  font-weight: 600;
}

.player{
  padding: 2em;
  background-color: #e2ebf1;
  border-radius: 25px;
  text-align:center;
}
h2.title{
  color:#535c69;
  text-align:center;
  font-size: 1.3em;
  line-height: 1.4em;
  padding-top:.8em;
}
.author{
  color:#616c7b;
  font-size: 16px;
  line-height: 1.2em;
  text-align:center;
}

.album{
  border: 6px solid #FFF;
  border-radius: 50%;
  box-shadow: 0 10px 10px rgba(0,0,0,.13);
}
.player img {
  margin: 0 auto;
}

.controls{
  display:flex;
  align-items:center;
  justify-content:center;
  margin-top:1em;
}

.prev,.next{
  background-color:#e9f3f9;
  padding: .4em .5em;
  border-radius: 50%;
  margin: .5em .5em 0 .5em;
  box-shadow: 0 10px 10px rgba(0,0,0,.13);
}

.play,.pause{
  background-color:#EA5923;
  padding: .5em .5em;
  border-radius: 50%;
  margin: .5em .5em 0 .5em;
  box-shadow: 0 10px 10px rgba(0,0,0,.13);
}
@media only screen and (max-width: 700px){
  main{
    flex-direction:column-reverse;

  }
  .playlist{
    margin-right:0;
  }
  .player{
    margin-bottom:20px;
  }
}

</style>