<template>
  <div class="game">
    <div class="game_title title">
      <div class="title_letters letters">
        <svg class="letters_img img" height="50" width="50">
          <line x1="0" y1="40" x2="40" y2="4" stroke="#db2d98" stroke-width="4" />
          <line x1="8" y1="2" x2="39" y2="49" stroke="#db2d98" stroke-width="4" />
        </svg>
        <svg class="letters_img img" height="50" width="50">
          <ellipse cx="25" cy="25" rx="18" ry="20" fill="none" stroke-width="4" stroke="#db2d98" />
        </svg>
        <svg
          class="letters_img img"
          fill="none"
          height="50"
          width="50"
          stroke="#db2d98"
          stroke-linecap="square"
          stroke-linejoin="square"
          stroke-width="1.8"
          viewBox="0 0 20 20"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path d="M0 0h24v24H0z" fill="none" stroke="none" />
          <path d="M5 19v.01" />
          <path d="M5 13v-10" />
        </svg>
      </div>
      <svg
        class="title_line line"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        x="0px"
        y="0px"
        width="120px"
        height="5px"
        xml:space="preserve"
      >
        <path class="path2" fill="none" stroke-width="6" stroke="#db2d98" d="M0 0 120 3" />
      </svg>
    </div>
    <GameBoard />
  </div>
</template>

<script setup>
import GameBoard from '@/components/GameBoard.vue'
</script>
<style>
.title {
  margin: 0px auto 60px;
  transition: transform 0.6s ease-out;
}
.title:hover {
  transform: rotate(10deg);
}
.title_line {
  transform: rotate(8deg);
  margin: -2px auto;
}
.title_letters {
  display: flex;
  justify-content: center;
  padding-left: 60px;
  text-align: center;
  font-size: 50px;
  color: #db2d98;
}
.letters_img {
  transform: rotate(5deg);
}
.letters_img:nth-child(2) {
  margin-left: -7px;
  transform: translateY(3px) rotate(17deg);
}
.letters_img:nth-child(3) {
  transform: translate(-4px, 3px) rotate(15deg);
}
</style>
