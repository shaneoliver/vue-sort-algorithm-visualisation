<template>
  <div id="app">
    <h1>Vue Sorting Algorithm Visualiser</h1>
    <p>Sorting strategy: Merge Sort</p>
    <DataVisualiser :points="points" />
    <button @click="changeNumberPoints(1)" class="button">Add Data</button>
    <button @click="changeNumberPoints(-1)" class="button">Remove Data</button>
    <button @click="createGraph" class="button">Randomise</button>
    <button @click="sort" class="button">Sort</button>
  </div>
</template>

<script>
import { getRandomInt } from "./random";
import { getMergeSortAnimations } from "./algorithms";
import DataVisualiser from "./components/DataVisualiser.vue";

// Change this value for the speed of the animations.
const ANIMATION_SPEED_MS = 10;

// This is the main color of the array bars.
const PRIMARY_COLOR = "cornflowerblue";

// This is the color of array bars that are being compared throughout the animations.
const SECONDARY_COLOR = "turquoise";

export default {
  name: "app",
  components: {
    DataVisualiser
  },
  data() {
    return {
      points: [],
      chart: {
        points: 50,
        min: 1,
        max: 1000
      },
      strategy: "merge-sort",
    };
  },
  mounted() {
    this.createGraph();
  },
  watch: {
    chart: {
      deep: true,
      handler() {
        this.createGraph();
      }
    }
  },
  methods: {
    createGraph() {
      this.points = [];
      for (let i = 0; i < this.chart.points; i++) {
        this.points.push(this.getPoint());
      }
    },
    changeNumberPoints(value) {      
      if(Math.sign(value) === -1) {
        this.points.splice(value, Math.abs(value));
        return
      }

      this.points.push(this.getPoint())
    },
    getPoint() {
      return getRandomInt(this.chart.min, this.chart.max)
    },
    sort() {
      let animations = [];

      switch (this.strategy) {
        case "merge-sort":
          animations = getMergeSortAnimations(this.points);
          break;
        default:
          animations = getMergeSortAnimations(this.points);
          break;
      }

      for (let i = 0; i < animations.length; i++) {
        const arrayBars = document.getElementsByClassName("bar");
        const isColorChange = i % 3 !== 2;
        if (isColorChange) {
          const [barOneIdx, barTwoIdx] = animations[i];
          const barOneStyle = arrayBars[barOneIdx].style;
          const barTwoStyle = arrayBars[barTwoIdx].style;
          const color = i % 3 === 0 ? SECONDARY_COLOR : PRIMARY_COLOR;
          setTimeout(() => {
            barOneStyle.backgroundColor = color;
            barTwoStyle.backgroundColor = color;
          }, i * ANIMATION_SPEED_MS);
        } else {
          setTimeout(() => {
            const [barOneIdx, newHeight] = animations[i];
            const barOneStyle = arrayBars[barOneIdx].style;
            barOneStyle.height = `${(newHeight / 1000) * 100}%`;
          }, i * ANIMATION_SPEED_MS);
        }
      }
    }
  }
};
</script>

<style>
* {
  box-sizing: border-box;
}
body,
html {
  padding: 0;
  margin: 0;
  background-color: gainsboro;
}
::selection {
  background-color: rgba(100, 149, 237, 50%);
  color: white;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
button {
  cursor: pointer;
  padding: 0.5rem 1rem;
  background-color: cornflowerblue;
  color: white;
  border-radius: 0.25rem;
  text-transform: uppercase;
  font-size: 1rem;
  letter-spacing: 0.15em;
  font-weight: 500;
  transition: all 0.3s ease;
  border: none;
}
button + button {
  margin-left: 1rem;
}
button:hover {
  background-color: #5681cd;
}
button:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(100, 149, 237, 50%);
}
</style>
