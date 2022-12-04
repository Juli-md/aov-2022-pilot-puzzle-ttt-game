<template>
  <div class="gameBoard">
    <GameBoardCell v-for="cell in grid" v-bind:key="cell.id" :cellData="cell" @fillIn="onFillCell" />
  </div>
  <p class="gameBoard__text text" v-if="isDraw">It's a draw!</p>
  <p class="gameBoard__text text" v-if="board.winner">
    <span class="text_letter letter">{{ board.winner }}</span> has won!
  </p>
  <p class="gameBoard__text text" v-if="!board.winner && !isDraw">
    It's <span class="text_letter letter">{{ board.players[board.currentPlayer] }}</span
    >'s turn.
  </p>
  <button
    class="gameBoard__btn btn"
    @click="refresh"
    :style="{ visibility: board.winner || isDraw ? 'visible' : 'hidden' }"
  >
    Play again
  </button>
</template>

<script setup>
import GameBoardCell from '@/components/GameBoardCell.vue'
import { reactive, ref, computed } from 'vue'

const FIELD_SIZE = 3
const board = reactive({
  cells: [],
  players: ['X', 'O'],
  moveCount: 0,
  currentPlayer: 0,
  winner: '',
})
board.cells = createBoard()

const grid = computed(() => {
  let cells = []
  return board.cells.flat(2)
})

// board
function createCell(index, position) {
  return {
    id: index,
    state: '',
    position,
  }
}

function createBoard() {
  let cells = []
  let index = 1
  for (let y = 0; y < FIELD_SIZE; y++) {
    cells.push([])
    for (let x = 0; x < FIELD_SIZE; x++) {
      cells[y].push(createCell(index, [y, x]))
      index++
    }
  }

  return cells
}

function refresh() {
  board.cells = createBoard()
  board.currentPlayer = 0
  board.moveCount = 0
  board.winner = ''
}

function changeCellValue([y, x]) {
  const currentCell = board.cells[y][x]
  currentCell.state = board.players[board.currentPlayer]
}

function changeCurrentPlayer() {
  board.currentPlayer = +!board.currentPlayer
}

function increaseMoveCount() {
  board.moveCount++
}

function isCellFilled({ state }) {
  return !!state
}

const isDraw = computed(() => {
  return !board.winner && board.moveCount === Math.pow(FIELD_SIZE, 2) - 1
})

const checkWinFnDict = {
  checkColumn:
    ([y, x]) =>
    counter =>
      board.cells[counter][x].state,
  checkRow:
    ([y, x]) =>
    counter =>
      board.cells[y][counter].state,
  checkRightDown:
    ([y, x]) =>
    counter =>
      board.cells[counter][counter].state,
  checkLeftDown:
    ([y, x]) =>
    counter =>
      board.cells[counter][FIELD_SIZE - 1 - counter].state,
}
function check(directionFn) {
  for (let i = 0; i < FIELD_SIZE; i++) {
    if (board.players[board.currentPlayer] !== directionFn(i)) {
      return false
    }
    if (i === FIELD_SIZE - 1) {
      console.log('WINNER')
      return true
    }
  }
}
function isWinnerFound({ position }) {
  let isFound = false
  for (const [fnName, checkWinFn] of Object.entries(checkWinFnDict)) {
    isFound = check(checkWinFn(position))
    if (isFound) return isFound
  }
  return isFound
}

function onFillCell(cellData) {
  if (isDraw.value) {
    return
  }
  //id, position
  if (isCellFilled(cellData)) {
    return
  }
  changeCellValue(cellData.position)
  increaseMoveCount()
  if (isWinnerFound(cellData)) {
    board.winner = board.players[board.currentPlayer]
  }

  // returns true
  // if winner - end , if draw - end if else - change player
  changeCurrentPlayer()
}
</script>

<style>
.gameBoard {
  width: 310px;
  height: 310px;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
  background: rgb(146, 45, 253);
  background: linear-gradient(0deg, rgba(146, 45, 253, 1) 0%, rgba(253, 45, 150, 1) 100%);
  grid-gap: 5px;
}
.gameBoard__text {
  margin-top: 10px;
}
.text {
  color: #db2d98;
  text-align: center;
  text-transform: uppercase;
  font-size: 16px;
  font-weight: bold;
}
.letter {
  font-size: 26px;
}
.gameBoard__btn-wrapper {
  height: 122px;
}
.gameBoard__btn {
  margin: 30px auto;
}
.btn {
  display: block;
  border: 4px solid rgb(146, 45, 253);
  color: #db2d98;
  padding: 15px 22px;
  text-align: center;
  text-transform: uppercase;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
}
</style>
