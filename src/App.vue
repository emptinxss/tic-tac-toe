<template>
  <div>
    <h1>TIC TAC TOE</h1>
    <Setup v-if="setup" :getSetup="getSetup" />
    <Board
      v-else
      :playerName="playerName"
      :opponentName="opponentName"
      :delHistory="delHistory"
      :reset="resetGame"
    />
  </div>
</template>

<script setup>
import { ref } from "@vue/reactivity";
import Board from "./components/Board.vue";
import Setup from "./components/Setup.vue";

let setup = ref(true);
let playerName = ref("");
let opponentName = ref("");
let delHistory = ref("");

const getSetup = (player, opponent, clearHistory) => {
  playerName.value = player;
  opponentName.value = opponent;
  delHistory.value = clearHistory;
  setup.value = false;
};
const resetGame = () => {
  setup.value = true;
};
</script>

<style>
@import "./assets/base.css";
@import "./assets/animation.css";

#app {
  margin: 0 auto;
  padding: 1rem;
  text-align: center;
}

#app h1 {
  font-weight: 700;
  word-spacing: 1px;
  letter-spacing: 0.5rem;
  font-size: 4rem;
  margin-bottom: 2rem;
  animation: 1s ease-out 0s 1 slideInFromTop;
}
</style>
