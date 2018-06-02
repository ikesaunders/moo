<template>
  <canvas @click="handleClick" ref="canvas" :height="height" :width="width">
  </canvas>
</template>

<script>
import Tone from 'tone';

class Square {
  constructor(x, y, width, height, context) {
    this.context = context
    this.height = height
    this.width = width
    this.x = x
    this.y = y
  }
  delete() {
    this.context.clearRect(this.x, this.y, this.width, this.height)
  }
}

const CMajScale = ['C3', 'D3', 'E3', 'F3', 'G3', 'A3', 'B3', 'C4']

export default {
  data () {
    return {
      columns: [],
      context: null,
      rows: [],
      squares: [],
      synth: new Tone.MonoSynth().toMaster(),
      columnWidth: this.width / 16,
      rowHeight: this.height / 7,
    }
  },
  mounted () {
    this.context = this.$refs.canvas.getContext('2d')
    this.createGrid()
  },
  methods: {
    createGrid() {

      var columns = [this.columnWidth]
      var rows = [this.rowHeight]
      this.context.lineWidth = 5
      this.context.strokeStyle = '#FFF'

      for (let index = 0; index < 14; index++) {
        columns.push(columns[index] + this.columnWidth)
      }
      for (let index = 0; index < 5; index++) {
        rows.push(rows[index] + this.rowHeight)
      }
      this.columns = columns
      this.rows = rows

      columns.forEach(x => {
        this.context.beginPath()
        this.context.moveTo(x, 0)
        this.context.lineTo(x, this.height)
        this.context.stroke()
      })

      rows.forEach(y => {
        this.context.beginPath()
        this.context.moveTo(0, y)
        this.context.lineTo(this.width, y)
        this.context.stroke()
      })

    },
    hasOverlappingCoordinates (square) {
      return (
        square.x <= this.mouseX && this.mouseX <= square.x + square.width &&
        square.y <= this.mouseY && this.mouseY <= square.y + square.height
      )
    },
    handleClick () {
      var overlappingSquareIndex = this.squares.findIndex(this.hasOverlappingCoordinates)
      if(overlappingSquareIndex !== -1) {
        this.removeSquare(overlappingSquareIndex)
      } else {
        this.addSquare({ x: this.mouseX, y: this.mouseY })
        this.draw()
      }
    },
    addSquare({ x, y }) {
      const nearestX = this.columns[this.columns.findIndex(point => x <= point) - 1]
      const nearestY = this.rows[this.rows.findIndex(point => y <= point) - 1]
      this.squares.push(new Square(nearestX, nearestY, this.columnWidth, this.rowHeight, this.context))
      const note = CMajScale[Math.round(((this.height - y) / this.height) * CMajScale.length) - 1]
      this.synth.triggerAttackRelease(note, "8n", undefined, y / this.width);
    },
    removeSquare(index) {
      this.squares[index].delete()
      this.squares.splice(index, 1)
      // this.squares = this.squares.reduce((squares, square) => {
      //   if(this.hasOverlappingCoordinates(square)) {
      //      square.delete()
      //      return squares
      //   } else {
      //     return squares.concat(square)
      //   }
      // }, [])
    },
    draw() {
      this.context.fillStyle = '#FFF37F'
      this.squares.map(square => {
        this.context.fillRect(square.x, square.y, square.width, square.height)
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
