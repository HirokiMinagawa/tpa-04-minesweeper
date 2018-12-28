<template>
  <div id="app">
    <button @click="startGame">Start Game</button>
    <table class="minesweeper">
      <tr v-for="(row, rowIndex) in classNames" :key="rowIndex">
        <Tile
          v-for="(col, colIndex) in row"
          :className="classNames[rowIndex][colIndex]"
          :rowIndex="rowIndex"
          :colIndex="colIndex"
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
      classNames: [],
      mineMap: [],
    };
  },
  components: {
    Tile: Tile
  },
  methods: {
    startGame: function() {
      this.initializeClassNames();
    },
    initializeClassNames: function() {
      const MAX_ROW = 10;
      const MAX_COL = 19;
      const classNames = [];
      const mineMap = [];
      for (let i = 0; i < MAX_ROW; i += 1) {
        classNames[i] = [];
        mineMap[i] = [];
        const classNamesRow = classNames[i];
        const mineMapRow = mineMap[i];
        for (let t = 0; t < MAX_COL; t += 1) {
          classNamesRow[t] = 'unopened';
          mineMapRow[t] = this.setMine();
        }
      }
      this.classNames =classNames;
      this.mineMap = mineMap;
    },
    setMine: function() {
      return Math.random() * 6 > 5;
    },
    openTile: function(tile) {
      //if  タイルが地雷なら(isMine())
      //  押されたタイルのクラスネームを"mine"に変更
      //  showAll():他のタイル全てに周りの地雷の数に応じたクラスネームを（地雷の場合は地雷を表示）
      //else タイルが地雷じゃない
      //  const mineCount = surroundedMineCount() count mine surrounded
      //  if 周辺に地雷がある時
      //     changeClassNameBasedMineCount() change ClassName Based on MineCount
      //   else 
      //     このタイルのclassNameを"opened"に
      //     再帰処理部分(if the neighbor has not been opened yet)
      //        (open the neighbor)
    },
    /**
     * isMine or not
     * @function
     * @param {object} tile
     * @return {boolean}
     */
    isMine: function(tile) {
      // this.mineMapでこのtileが地雷かどうか調べる
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
      //      周りの地雷の個数に応じてクラスネームを変更 surroundedMineCount() , changeClassNameBasedMineCount()
    },
    /**
     * count mine surrounded
     * @function
     * @param {Object} tile
     * @return {number}
     */
    surroundedMineCount: function(tile){
      // this.mineMapを使って周辺の地雷の数を調べる
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
    changeClassNameBasedMineCount: function(row, col, mineCount) {
      // 地雷の数に応じてクラスネームを変更
    },
    setFlag: function(tile) {
      this.classNames[tile.rowIndex].splice(tile.colIndex, 1, 'flagged');
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
