<template>
  <div id="app" @mousemove="handleMouseMove">
    <Canvas :height="browserHeight" :width="browserWidth" :mouseX="mouseX" :mouseY="mouseY"/>
  </div>
</template>

<script>
import Canvas from './components/Canvas.vue'

export default {
  name: 'app',
  components: {
    Canvas
  },
  data: function() {
    return {
      browserHeight: window.innerHeight,
      browserWidth: window.innerWidth,
      mouseX: 0,
      mouseY: 0,
    }
  },
  methods: {
    handleMouseMove (e) {
      this.mouseX = e.x
      this.mouseY = e.y
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  background: linear-gradient(90deg, #67B26F, #4ca2cd);
}
body {
  margin: 0;
  overflow: hidden;
  height: 100vh;
}
</style>
