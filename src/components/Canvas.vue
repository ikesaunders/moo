<template>
  <canvas @click="handleClick" ref="canvas" :height="height" :width="width">
  </canvas>
</template>

<script>
import Tone from 'tone';

class Square {
  constructor(x, y, context) {
    this.x = x
    this.y = y
    this.context = context
  }
  delete() {
    this.context.clearRect(this.x - 40, this.y - 40, 80, 80)
  }
}

const CMajScale = ['C3', 'D3', 'E3', 'F3', 'G3', 'A3', 'B3', 'C4']

export default {
  data () {
    return {
      context: null,
      squares: [],
      synth: new Tone.MonoSynth().toMaster(),
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
      if(overlappingSquares.length) {
        this.removeOverlappingSquares()
      } else {
        this.addSquare({ x: this.mouseX, y: this.mouseY })
        this.draw()
      }
    },
    addSquare({ x, y }) {
      this.squares.push(new Square(x, y, this.context))
      const note = CMajScale[Math.round(((this.height - y) / this.height) * CMajScale.length) - 1]
      this.synth.triggerAttackRelease(note, "8n", undefined, y / this.width);
    },
    removeOverlappingSquares() {
      this.squares = this.squares.reduce((squares, square) => {
        if(this.hasOverlappingCoordinates(square)) {
           square.delete()
           return squares
        } else {
          return squares.concat(square)
        }
      }, [])
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
