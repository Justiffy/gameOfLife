<template>
  <div class="hello">
    <button @click="start">Start</button>
    <div v-for="(items, index) in matrix" :key=index class="column">
      <div v-for="(item, index) in items" :key=index class="row">
        <div class="cells" :class="{ life: item }"></div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'GameOfLife',
  data() {
    return {
      matrix: [
        [0, 0, 1, 0, 0, 0, 0, 0, 0, 0],
        [1, 0, 1, 0, 0, 0, 0, 0, 0, 0],
        [0, 1, 1, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      ],
      newMatrix: [
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      ],
    };
  },
  methods: {
    start() {
      setInterval(this.nextTick, 250);
    },
    nextTick() {
      const matrix = this.matrix;
      for (let i = 0; i < this.matrix.length; i += 1) {
        for (let j = 0; j < this.matrix[i].length; j += 1) {
          this.newMatrix[i][j] = this.checkOnLife(i, j, matrix[i][j]);
        }
      }
      this.matrix = JSON.parse(JSON.stringify(this.newMatrix));
    },
    checkOnLife(i, j, val) {
      const matrix = this.matrix;
      const iplus = (matrix[i + 1] === undefined) ? 0 : i + 1;
      const iminus = (matrix[i - 1] === undefined) ? matrix.length - 1 : i - 1;
      const jplus = (matrix[i][j + 1] === undefined) ? 0 : j + 1;
      const jminus = (matrix[i][j - 1] === undefined) ? matrix[i].length - 1 : j - 1;

      let summ = 0;
      summ = (matrix[iminus][jminus] === 1) ? summ += 1 : summ;
      summ = (matrix[iminus][j] === 1) ? summ += 1 : summ;
      summ = (matrix[iminus][jplus] === 1) ? summ += 1 : summ;
      summ = (matrix[i][jminus] === 1) ? summ += 1 : summ;
      summ = (matrix[i][jplus] === 1) ? summ += 1 : summ;
      summ = (matrix[iplus][jminus] === 1) ? summ += 1 : summ;
      summ = (matrix[iplus][j] === 1) ? summ += 1 : summ;
      summ = (matrix[iplus][jplus] === 1) ? summ += 1 : summ;
      if (val === 0) {
        return (summ === 3) ? 1 : 0;
      }
      return (summ === 2 || summ === 3) ? 1 : 0;
    },
  },
};
</script>

<style scoped>
.column {
  display: flex;
  flex-direction: row;
}
.cells {
  width: 15px;
  height: 15px;
  margin: 5px;
  background-color: rgb(241, 241, 241);
}
.life {
  background-color: rgb(126, 126, 126);
}
.hello {
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
} 
</style>
