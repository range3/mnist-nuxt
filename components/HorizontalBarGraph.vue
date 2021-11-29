<template>
  <v-card tile class="horizontal-bar-graph d-flex flex-column">
    <v-card
      v-for="(v, index) in value"
      :key="index"
      tile
      class="d-flex"
    >
      <div>{{ index }}</div>
      <horizontal-bar-graph-item
        :value="v"
      />
    </v-card>
  </v-card>
</template>

<script>
export default {
  props: {
    value: {
      type: Array,
      default: Array(10).fill(0)
    }
  }
}
</script>
