<template>
  <div class="spiral">
    <h1 v-if="count > 0 && count % 1 === 0">Count: {{ count }}</h1>
    <h1 v-else>Please insert a whole number greater than 0</h1>
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
      numbers: this.getMatrix(),
    };
  },
  methods: {
    numberBackgroundColor(count, i) {
      var style = {};
      const step = 250 / Math.pow(count * count, 0.47),
        val = i * step;

      style.backgroundColor = "hsl(" + val + ", 100%, 50%)";

      return style;
    },
    getNumbers(matrix, count) {
      let direction = "right";

      let x = 0,
        y = 0;

      for (let i = 1; i < count * count; ++i) {
        if (direction == "right") {
          if (x + 1 < count && !matrix[y][x + 1]) {
            x++;
          } else {
            direction = "down";
          }
        }
        if (direction == "down") {
          if (y + 1 < count && !matrix[y + 1][x]) {
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
    },
    getNumbersRecursive(matrix, direction, degree, currentPos, current, count) {
      if (current > count * count - 1) return;
      const x = Math.round(Math.cos(degree)),
        y = Math.round(Math.sin(degree));

      while (
        currentPos[0] + direction[0] < count && // Right Edge
        currentPos[1] + direction[1] < count && // Bottom Edge
        currentPos[0] + direction[0] >= 0 && // Left Edge
        currentPos[1] + direction[1] >= 0 && // Top edge
        !matrix[currentPos[1] + direction[1]][currentPos[0] + direction[0]] // Make sure next element is null
      ) {
        current++;
        currentPos[0] += direction[0];
        currentPos[1] += direction[1];
        matrix[currentPos[1]][currentPos[0]] = current;
      }

      direction[0] = x;
      direction[1] = y;

      if (degree == Math.PI * 2) {
        degree = 0;
      } else degree += Math.PI / 2;

      this.getNumbersRecursive(
        matrix,
        direction,
        degree,
        currentPos,
        current,
        count
      );
    },
    getMatrix() {
      if (isNaN(this.count) || this.count % 1 != 0 || this.count < 1) return;
      let matrix = new Array();

      for (let j = 0; j < this.count; ++j) {
        const row = new Array();
        for (let i = 0; i < this.count; ++i) {
          row[i] = null;
        }
        matrix.push(row);
      }

      matrix[0][0] = 1;

      //this.getNumbers(matrix, this.count);

      this.getNumbersRecursive(
        matrix,
        new Array(1, 0),
        0,
        new Array(0, 0),
        1,
        this.count
      );

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
