<template>
  <div class="data-visualiser">
    <div class="chart">
      <div
        class="bar"
        v-for="(point, index) in points"
        :key="index"
        :style="`height: ${getBarHeight(point)};`"
        ref="bar"
      ></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Chart",
  props: {
    points: Array,
    default: []
  },
  methods: {
    getBarHeight(value) {
      return `${value / 1000 * 100}%`;
    },
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.data-visualiser {
  padding: 2rem;
}
.chart {
  display: flex;
  align-items: flex-end;
  justify-content: center;
  height: 600px;
  padding: 2rem;
  border-radius: 0.5rem;
  background-color: white;
  box-shadow: 0 10px 30px rgba(100, 149, 237, 0.1),
    0 7px 12px rgba(100, 149, 237, 0.15), 0 2px 5px rgba(0, 0, 0, 0.1);
}
.bar {
  background-color: cornflowerblue;
  width: 100%;
  height: 1rem;
  margin: 0 0.25rem;
  transition: height 0.3s ease;
}
</style>
