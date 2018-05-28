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
    this.clear()
  },
  methods: {
    hasOverlappingCoordinates (square) {
      return (
        square.x - 40 <= this.mouseX && this.mouseX <=square.x + 40 &&
        square.y - 40 <= this.mouseY && this.mouseY <=square.y + 40
      )
    },
    handleClick () {
      var overlappingSquares = this.squares.filter(this.hasOverlappingCoordinates)
      console.log('overlappingSquares', overlappingSquares);
      if(overlappingSquares.length) {
        this.removeOverlappingSquares()
      } else {
        this.addSquare({ x: this.mouseX, y: this.mouseY })
      }
      console.log('this.squares', this.squares);
      this.clear()
      this.draw()
    },
    addSquare({ x, y }) {
      this.squares.push(new Square(x, y))
      this.synth.triggerAttackRelease(x, "8n");
    },
    removeOverlappingSquares() {
      this.squares = this.squares.filter(square => !this.hasOverlappingCoordinates(square))
    },
    draw() {
      this.context.fillStyle = '#FFF37F'
      this.squares.map(square => {
        this.context.fillRect(square.x - 40, square.y - 40, 80, 80)
        }
      )
    },
    clear() {
      this.context.clearRect(0, 0, this.width, this.height)
    }
  },
  name: 'Canvas',
  props: ['height', 'width', 'mouseX', 'mouseY']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
