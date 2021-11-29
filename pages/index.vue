<template>
  <v-row justify="center" align="center">
    <v-col>
      <v-card class="d-flex justify-center">
        <horizontal-bar-graph class="d-flex justify-start" :value="output" />
        <v-card>
          <drawing-canvas ref="canvas" @onChange="onCanvasChange" />
          <v-card-actions>
            <v-btn text elevation="2" @click="clearCanvas($event)">
              Clear
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-card>
      <canvas ref="canvasResized" class="canvas-resized" width="28" height="28" />
    </v-col>
  </v-row>
</template>

<script>
import { env } from 'onnxruntime-common'
// undocumented feature
env.wasm.wasmPaths = process.env.STATIC_BASE + '/'
// eslint-disable-next-line
import { InferenceSession, Tensor } from 'onnxruntime-web'

export default {
  data () {
    return {
      session: null,
      input: null,
      output: Array(10).fill(0)
    }
  },
  async mounted () {
    this.session = await InferenceSession.create(`${process.env.STATIC_BASE}/mnist_cnn_13.onnx`)
  },
  methods: {
    clearCanvas () {
      this.$refs.canvas.clear()
    },
    onCanvasChange (canvas) {
      const tensor = this.preprocess(canvas)
      this.session.run({ input: tensor }).then((result) => {
        this.output = Array.from(result.output.data).map(logx => Math.exp(logx))
      })
    },
    preprocess (canvas) {
      const data = this.resizeImgData(canvas).data

      // convert the image shape from [canvas.width, canvas.height, 4] to [1, 28, 28]
      const input = new Float32Array(784)
      for (let i = 0, len = data.length; i < len; i += 4) {
        input[i / 4] = data[i + 3] / 255.0 // use alpha channel
      }

      // const tensor = new Tensor('float32', this.normalize(input), [1, 1, 28, 28])
      const tensor = new Tensor('float32', input, [1, 1, 28, 28])
      return tensor
    },
    resizeImgData (canvas) {
      // const cxt = canvas.getContext('2d')
      // ctx.getImageData(0, 0, ctx.canvas.width, ctx.canvas.height)
      const resizeCanvas = this.$refs.canvasResized
      const resizeCxt = resizeCanvas.getContext('2d')

      // draw scaled image
      resizeCxt.clearRect(0, 0, 28, 28)
      resizeCxt.drawImage(canvas, 0, 0, canvas.width, canvas.height, 0, 0, 28, 28)

      // return data
      return resizeCxt.getImageData(0, 0, 28, 28)
    },
    normalize (input) {
      const transforms = [[0.1307], [0.3081]]
      for (let i = 0, len = input.length; i < len; i += 1) {
        input[i] = (input[i] - transforms[0][0]) / transforms[1][0]
      }
      return input
    }
  }
}
</script>

<style scoped>
.canvas-resized {
  background-color: #fff;
}
</style>
