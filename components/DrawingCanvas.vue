<template>
  <div class="drawing-canvas-wrap">
    <canvas
      ref="canvas"
      class="drawing-canvas"
      width="320px"
      height="320px"
      @mousedown="dragStart"
      @mouseup="dragEnd"
      @mouseout="dragEnd"
      @mousemove="draw"
    />
  </div>
</template>

<script>
export default {
  data () {
    return {
      isDrag: false
    }
  },
  mounted () {
    this.canvas = this.$refs.canvas
    this.context = this.canvas.getContext('2d')
    this.context.lineCap = 'round'
    this.context.lineJoin = 'round'
    this.context.lineWidth = 20
    this.context.strokeStyle = '#000000'
  },
  methods: {
    draw (e) {
      const x = e.layerX
      const y = e.layerY

      if (!this.isDrag) {
        return
      }

      this.context.lineTo(x, y)
      this.context.stroke()
      this.$emit('onChange')
    },
    dragStart (e) {
      const x = e.layerX
      const y = e.layerY

      this.context.beginPath()
      this.context.lineTo(x, y)
      this.context.stroke()

      this.isDrag = true
      this.$emit('onChange')
    },
    dragEnd (e) {
      this.context.closePath()
      this.isDrag = false
    },
    clear () {
      this.context.clearRect(0, 0, this.canvas.width, this.canvas.height)
      this.$emit('onChange')
    }
  }
}
</script>

<style scoped>
.drawing-canvas-wrap {
  width: 320px;
}
.drawing-canvas {
  border: 1px solid #000;
  background-color: #fff;
}
</style>
