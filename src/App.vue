<template>
  <div id="app">
    <button @click="startGame">Start Game</button>
    <table class="minesweeper">
      <tr v-for="(row, rowIndex) in tiles" :key="rowIndex">
        <Tile
          v-for="(col, colIndex) in row"
          :status="col"
          :key="colIndex"
          @clickedRight="setFlag"
          @clickedLeft="openTile"
        ></Tile>
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
      tiles: [],
    };
  },
  components: {
    Tile,
  },
  methods: {
    startGame: function() {
      this.initializeTiles();
    },
    initializeTiles: function() {
      const MAX_ROW = 10;
      const MAX_COL = 19;
      const tiles = [];
      for (let i = 0; i < MAX_ROW; i += 1) {
        const row = i;
        tiles[row] = [];
        for (let t = 0; t < MAX_COL; t += 1) {
          const col = t;
          tiles[row][col] = this.initializeTileStatus(row, col);
        }
      }
      this.tiles = tiles;
    },
    initializeTileStatus: function(row, col) {
      const tileStatus = {};
      tileStatus.state = 'unopened';
      tileStatus.row = row;
      tileStatus.col = col;
      tileStatus.mined = Math.random() * 6 > 5;
      return tileStatus;
    },
    /**
     * open tile
     * @function
     * @param {object} tileStatus
     * @return {boolean}
     */
    openTile: function(tileStatus) {
      if (tileStatus.mined) {
        this.showAll();
      } else {
        const mineCount = this.surroundedMineCount(tileStatus.row, tileStatus.col);
        this.changeClassNameBasedMineCount(tileStatus.row, tileStatus.col, mineCount);
        if (mineCount === 0) {
          // 再帰処理部分(if the neighbor has not been opened yet)
          // (open the neighbor)
        }
      }
    },
    /**
     * show all tile
     * @function
     * @return {undefined}
     */
    showAll: function() {
      for (let i = 0, len = this.tiles.length; i < len; i += 1) {
        const row = i;
        for (let t = 0, len = this.tiles[row].length; t < len; t += 1) {
          const col = t;
          const tileStatus = this.tiles[row][col];
          if (tileStatus.mined) {
            tileStatus.state = 'mine';
          } else {
            const mineCount = this.surroundedMineCount(row, col);
            this.changeClassNameBasedMineCount(row, col, mineCount);
          }
        }
      }
    },
    /**
     * count mine surrounded
     * @function
     * @param {number} row
     * @param {number} col
     * @return {number}
     */
    surroundedMineCount: function(row, col){
      let mineCount = 0;
      const surroundedPosition = [
        [-1, -1], [-1, 0], [-1, 1],
        [0, -1], [0, 1],
        [1, -1], [1, 0], [1, 1],
      ];
      for (let i = 0; i < surroundedPosition.length; i++) {
        const rowChecking = row + surroundedPosition[i][0];
        const colChecking = col + surroundedPosition[i][1];
        if (this.isValidTile(rowChecking, colChecking)) continue;
        if (this.tiles[rowChecking][colChecking].mined) {
          mineCount += 1;
        }
      }
      return mineCount;
    },
    /**
     * valid tile
     * @function
     * @param {number} row
     * @param {number} col
     * @return {boolean}
     */
    isValidTile(row, col) {
      return !(this.tiles[row] && this.tiles[row][col]);
    },
    /**
     * change ClassName Based on MineCount
     * @function
     * @param {number} row
     * @param {number} col
     * @param {number} mineCount
     * @return {undefined}
     */
    changeClassNameBasedMineCount: function(row, col, mineCount) {
      this.tiles[row][col].state = `mine-neighbor-${mineCount}`;
      if (mineCount == 0) this.tiles[row][col].state = 'opened';
    },
    setFlag: function(tileStatus) {
      this.tiles[tileStatus.row][tileStatus.col].state = 'flagged';
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
