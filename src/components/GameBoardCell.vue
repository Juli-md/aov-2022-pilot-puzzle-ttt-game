<template>
  <div class="gameBoard_cell cell" @click="fill">
    <svg class="cell_img img" height="50" width="50" v-if="showPlayer === 'O'">
      <circle cx="25" cy="25" r="20" fill="none" stroke-width="4" stroke="#db2d98" />
    </svg>
    <svg class="cell_img img" height="50" width="50" v-if="showPlayer === 'X'">
      <line x1="7" y1="40" x2="40" y2="7" stroke="#db2d98" stroke-width="4" />
      <line x1="7" y1="7" x2="40" y2="40" stroke="#db2d98" stroke-width="4" />
    </svg>
  </div>
</template>

<script setup>
import { reactive, ref, computed } from 'vue'

const props = defineProps({
  cellData: Object,
})

const emit = defineEmits(['fillIn'])

function fill() {
  emit('fillIn', props.cellData)
}

const showPlayer = computed(() => {
  return props.cellData.state
})
</script>

<style>
.gameBoard_cell {
  display: flex;
  width: 100px;
  height: 100px;
  background: rgba(21, 9, 34, 1);
}
.cell_img {
  margin: auto;
}
</style>
