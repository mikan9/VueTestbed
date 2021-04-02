<template>
  <div class="spiral">
    <h1>Count: {{ count }}</h1>
    <div id="matrix">
      <div class="row" v-for="row in numbers" :key="row.index">
        <div
          v-bind:style="
            numberBackgroundColor(numbers.length * row.length, number)
          "
          class="number"
          v-for="number in row"
          :key="number.index"
        >
          {{ number }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Spiral",
  props: {
    count: Number,
  },
  data() {
    return {
      numbers: this.renderMatrix(),
    };
  },
  methods: {
    numberBackgroundColor(count, i) {
      var style = {};
      style.backgroundColor = "#ffff00";
      const step = count / 220,
        val = i * step;

      style.filter = "brightness(" + (100 - val) + "%)";

      return style;
    },
    renderMatrix() {
      const matrix = new Array();
      let direction = "right";

      for (let j = 0; j < this.count; ++j) {
        const row = new Array();
        for (let i = 0; i < this.count; ++i) {
          row[i] = null;
        }
        matrix.push(row);
      }

      let x = 0,
        y = 0;

      matrix[0][0] = 1;

      for (let i = 1; i < this.count * this.count; ++i) {
        if (direction == "right") {
          if (x + 1 < this.count && !matrix[y][x + 1]) {
            x++;
          } else {
            direction = "down";
          }
        }
        if (direction == "down") {
          if (y + 1 < this.count && !matrix[y + 1][x]) {
            y++;
          } else {
            direction = "left";
          }
        }
        if (direction == "left") {
          if (x - 1 >= 0 && !matrix[y][x - 1]) {
            x--;
          } else {
            direction = "up";
          }
        }
        if (direction == "up") {
          if (y - 1 >= 0 && !matrix[y - 1][x]) {
            y--;
          } else {
            direction = "right";
            x++;
          }
        }
        matrix[y][x] = i + 1;
      }
      return matrix;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

#matrix {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.row {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}

.number {
  display: flex;
  justify-content: center;
  align-items: center;

  width: 2em;
  height: 2em;
  border: 1px #000 solid;
}
</style>
