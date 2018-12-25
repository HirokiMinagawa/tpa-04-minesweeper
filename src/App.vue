<template>
  <div id="app">
    <button @click="startGame">Start Game</button>
    <table class="minesweeper">
      <tr v-for="(row, rowIndex) in rows" :key="rowIndex">
        <Tile v-for="(col, colIndex) in cols" :className="classNames[rowIndex][colIndex]" :key="colIndex"></Tile>
      </tr>
    </table>
  </div>
</template>

<script>
import Tile from './components/Tile.vue';

export default {
  name: 'App',
  data: () => {
    return {
      rows: 0,
      cols: 0,
      classNames: [],
    };
  },
  components: {
    Tile: Tile,
  },
  methods: {
    startGame: function() {
      this.rows = 10;
      this.cols = 19;
      this.initializeClassNames();
    },
    initializeClassNames: function() {
      for (let i = 0; i < this.rows; i += 1) {
        this.classNames[i] = [];
        const classNamesRow = this.classNames[i];
        for (let t = 0; t < this.cols; t += 1) {
          classNamesRow[t] = 'unopened';
        }
      }
    },
  },
};
</script>

<style>
body {
  font-family: Helvetica, sans-serif;
  background: #333;
  color: #fff;
}

button {
  margin: 20px auto;
  display: block;
}

table.minesweeper {
  margin: auto;
  border: 1px solid #999;
  border-collapse: collapse;
  background-color: #ddd;
}

table.minesweeper td {
  width: 24px;
  height: 24px;
  background-size: cover;
}
</style>
