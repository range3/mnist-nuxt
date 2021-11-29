<template>
  <v-row justify="center" align="center">
    <v-col>
      <v-card class="d-flex justify-center">
        <horizontal-bar-graph class="d-flex justify-start" />
        <v-card>
          <drawing-canvas ref="canvas" @onChange="onCanvasChange" />
          <v-card-actions>
            <v-btn text elevation="2" @click="clearCanvas">
              Clear
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  methods: {
    clearCanvas () {
      this.$refs.canvas.clear()
    },
    onCanvasChange () {
      console.log('onCanvasChange')
    }
  }
}
</script>
