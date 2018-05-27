<template>
  <canvas @click="handleClick" ref="canvas" :height="height" :width="width">
  </canvas>
</template>

<script>
import Tone from 'tone';

class Square {
  constructor(x, y, index) {
    this.x = x
    this.y = y
    this.index = index
  }
}

export default {
  data () {
    return {
      context: null,
      squares: [],
      synth: new Tone.Synth().toMaster(),
    }
  },
  mounted () {
    this.context = this.$refs.canvas.getContext('2d')
    var gradient = this.context.createLinearGradient(0,0,this.width,0)
    gradient.addColorStop(0,"#3494E6");
    gradient.addColorStop(1,"#EC6EAD");
    this.context.fillStyle=gradient;
    this.context.fillRect(0, 0, this.width, this.height);
  },
  methods: {
    doesContain (square) {
      return (
        square.x - 10 <= this.mouseX && this.mouseX <=square.x + 10 &&
        square.y - 10 <= this.mouseY && this.mouseY <=square.y + 10
      )
    },
    handleClick () {
      var preexistingSquare = this.squares.find(this.doesContain)
      if(preexistingSquare) {
        this.removeSquare(preexistingSquare)
      } else {
        this.addSquare({ x: this.mouseX, y: this.mouseY })
      }
    },
    addSquare({ x, y }) {
      var newSquare = new Square(x, y, this.squares.length - 1)
      this.squares.push(newSquare)
      this.context.fillStyle = '#FFF37F';
      this.context.fillRect(newSquare.x - 10, newSquare.y - 10, 20, 20);
      this.synth.triggerAttackRelease(x, "8n");
    },
    removeSquare(preexistingSquare) {
      this.squares.splice(preexistingSquare.index, 1)
      this.context.clearRect( preexistingSquare.x - 10, preexistingSquare.y - 10, 20, 20)
    }
  },
  name: 'Canvas',
  props: ['height', 'width', 'mouseX', 'mouseY']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
