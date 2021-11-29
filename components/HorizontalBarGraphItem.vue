<template>
  <div class="horizontal-bar-graph-item">
    <div class="horizontal-bar-graph-item-inner" :style="innerStyle" />
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: Number,
      default: 0
    }
  },
  computed: {
    innerStyle () {
      return {
        width: `${this.value * 100}%`
      }
    }
  }
}
</script>

<style scoped>
.horizontal-bar-graph-item {
  width: 202px;
  height: 22px;
  margin: 0 0 3px 0;
  padding: 0;
  border: 1px solid #000;
  background-color: #fff;
}

.horizontal-bar-graph-item-inner {
  height: 20px;
  margin: 0;
  padding: 0;
  border: none;
  background-color: #888;
}
</style>
