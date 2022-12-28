<template>
  <div class="btn-container">
    <span id="visualize" class="btn btn-start" @click="runDijkstraAlgo">Go</span>
    <span id="resetBtn" class="btn btn-reset" @click="reset">Reset</span>
  </div>
  <div>
    <div
      v-for="(row, rowIndex) in grid"
      :key="rowIndex"
      class="grid"
    >
      <div
        v-for="(col, colIndex) in row"
        :key="colIndex"
        :id="`node-${col.row}-${col.col}`"
        class="node"
        :class="[
            col.isFinish ? 'finish-node' : '',
            col.isStart ? 'start-node' : '',
          ]"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import {onBeforeMount, reactive, ref} from "vue";
import { dijkstra, findTheShortestPath } from "./dijkstra";

onBeforeMount(() => {
  getInitialGrid();
});

const grid = [];
const startNode =  {
  row: 1,
  col: 1,
  element: null,
};
const finishNode = {
  row: 1,
  col: 6,
  element: null,
};

const getInitialGrid = () => {
  for (let row = 0; row < 8; row++) {
    const currentRow = [];
    for (let col = 0; col < 8; col++) {
      currentRow.push(createNode(col, row));
    }
    grid.push(currentRow);
  }
};

const createNode = (col: number, row: number) => {
  return {
    col,
    row,
    isStart: row === startNode.row && col === startNode.col,
    isFinish: row === startNode.row && col === finishNode.col,
    isVisited: false,
    previousNode: null,
    distance: Infinity,
  };
};

const runDijkstraAlgo = () => {
  const start = grid[startNode.row][startNode.col];
  const finish = grid[finishNode.row][finishNode.col];
  dijkstra(grid, start, finish);
  findTheShortestPath(finishNode);
}

</script>

<style lang="scss">
.btn-container {
  display: flex;
  gap: 1rem;
  margin-bottom: 1rem;
}

.btn {
  padding: 0.45em 0.7em;
  border-radius: 0.3em;
  color: #111;
  font-size: 0.95em;
  box-sizing: border-box;
  max-height: 2.5em;
  user-select: none;
}

.btn-start {
  background: #2DFE54;
}

.btn-reset {
  background: #FD4659;
}

.grid {
  display: flex;
  justify-content: center;
  align-items: center;
}

.node {
  height: 25px;
  width: 25px;
  outline: 1px solid rgba($color: #343f56, $alpha: 0.5);
  display: inline-block;

  &.finish-node {
    background: transparent;
    position: relative;

    &::before {
      content: "";
      position: absolute;
      left: 0;
      background: #FD4659;
      width: 100%;
      height: 100%;
      cursor: grab;
    }
  }

  &.start-node {
    position: relative;
    background: transparent;

    &::before {
      content: "";
      position: absolute;
      left: 0;
      background: #2DFE54;
      width: 100%;
      height: 100%;
      cursor: grab;
    }
  }
}
</style>
