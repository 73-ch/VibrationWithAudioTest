<template>
  <div id="app">
    <button id="start" @click="start">start</button>
  </div>
</template>

<script>
import {Filter, Player, FFT} from "tone";

export default {
  name: 'App',
  methods: {
    start() {
      const player = new Player("./test40.wav");
      // play as soon as the buffer is loaded
      player.autostart = true;
      player.loop = true;

      const boundary_freq = 80;
      console.log(player.context);

      const audio_filter = new Filter(boundary_freq, 'highpass', -96);
      const vibration_filter = new Filter(boundary_freq, 'lowpass', -96);

      player.connect(audio_filter);
      player.connect(vibration_filter);

      audio_filter.toDestination();

      const fft = new FFT();
      vibration_filter.connect(fft);
      // let before = new Float32Array(1024);

      let beforeAt = 0;

      setInterval(() => {
        // console.log(fft.getValue()[0]);

        if (fft.getValue()[0] > -40  && player.context.currentTime - beforeAt > 0.2) {
          beforeAt = player.context.currentTime;
          console.log("bang");
        }
      },10);
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
