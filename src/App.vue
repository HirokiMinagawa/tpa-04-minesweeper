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
    openTile: function() {
      //if  タイルが地雷なら(tileStatus.mined)
      //  押されたタイルのクラスネームを"mine"に変更
      //  showAll():他のタイル全てに周りの地雷の数に応じたクラスネームを（地雷の場合は地雷を表示）
      //else タイルが地雷じゃない
      //  const mineCount = surroundedMineCount(tileStatus.row, tileStatus.col)
      //  if 周辺に地雷がある時(mineCount > 0)
      //     changeClassNameBasedMineCount(tileStatus.row, tileStatus.col, mineCount);
      //   else 
      //     このタイルのclassNameを"opened"に
      //     再帰処理部分(if the neighbor has not been opened yet)
      //        (open the neighbor)
    },
    /**
     * show all tile
     * @function
     * @return {undefined}
     */
    showAll: function() {
      //for 行
      //  for 列
      //    if 地雷なら
      //      クラスネームを"mine"に変更
      //    else
      //      周りの地雷の個数に応じてクラスネームを変更 surroundedMineCount, changeClassNameBasedMineCount
    },
    /**
     * count mine surrounded
     * @function
     * @param {number} row
     * @param {number} col
     * @return {number}
     */
    surroundedMineCount: function(){
      // tilesを使って周辺の地雷の数を調べる
      // return mineCount;
    },
    /**
     * change ClassName Based on MineCount
     * @function
     * @param {number} row
     * @param {number} col
     * @param {number} mineCount
     * @return {undefined}
     */
    changeClassNameBasedMineCount: function() {
      // 地雷の数に応じてクラスネームを変更
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
