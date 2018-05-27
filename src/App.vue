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
      browserHeight: 0,
      browserWidth: 0,
      mouseX: 0,
      mouseY: 0,
    }
  },
  mounted: function() {
    this.browserHeight = window.innerHeight;
    this.browserWidth = window.innerWidth;
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
}
body {
  margin: 0;
}
</style>
