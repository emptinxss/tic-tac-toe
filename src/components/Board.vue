<template>
  <div>
    <h2 class="turn" v-if="!winner && !tie">
      <span :class="` ${player === 'X' ? 'x-color' : 'o-color'}`">
        {{
          player === "X" ? playerName : player === "O" ? opponentName : "none"
        }}
      </span>
      's turn
    </h2>

    <h2 :class="`wins ${winner === 'X' ? 'x-color' : 'o-color'}`" v-if="winner">
      <span>
        {{
          winner === "X" ? playerName : winner === "O" ? opponentName : "none"
        }}
      </span>
      <span> wins </span>
    </h2>
    <h2 class="wins tie" v-if="tie && !winner"><span>TIE</span></h2>

    <div class="container">
      <div class="board">
        <div class="row" v-for="(row, x) in boards" :key="x">
          <div
            v-for="(cell, y) in row"
            :key="y"
            @click="addMove(x, y)"
            :class="`column cell ${cell === 'X' ? 'x-color' : 'o-color'}`"
          >
            {{ cell === "X" ? "X" : cell === "O" ? "O" : "" }}
          </div>
        </div>
      </div>

      <div class="c-right" v-if="winner">
        <h2>history</h2>
        <div class="history" v-for="(winner, idx) in history" :key="idx">
          Game: {{ idx + 1 }}:
          <span :class="`${winner === 'X' ? 'x-color' : 'o-color'}`">
            {{
              winner /*  === "X" ? playerName : winner === "O" ? opponentName : "none" */
            }}</span
          >
          won
        </div>
      </div>
    </div>

    <div>
      <button class="btn" @click="reset">Reset</button>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "@vue/reactivity";
import { onMounted, watch } from "@vue/runtime-core";

export default {
  props: ["reset", "playerName", "opponentName", "delHistory"],
  setup(props) {
    const player = ref("X");
    const boards = ref([
      ["", "", ""],
      ["", "", ""],
      ["", "", ""],
    ]);

    const calculateWinner = (squares) => {
      const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ];
      for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i];
        if (
          squares[a] &&
          squares[a] === squares[b] &&
          squares[a] === squares[c]
        ) {
          return squares[a];
        }
      }
      return null;
    };

    const winner = computed(() => calculateWinner(boards.value.flat()));

    const tie = computed(() => isCat());

    function isCat() {
      let isTie = true;
      boards.value.flat().forEach((s) => {
        if (s === "") isTie = false;
      });
      if (isTie === true) return true;
    }

    const addMove = (x, y) => {
      //return winner
      if (winner.value) return;

      //return tie
      if (tie.value) return;

      //check override player's sign
      if (boards.value[x][y] !== "") return;

      boards.value[x][y] = player.value;
      player.value = player.value === "X" ? "O" : "X";
    };

    const history = ref([]);
    watch(winner, (current, previous) => {
      if (current && !previous) {
        history.value.push(current);
        localStorage.setItem("history", JSON.stringify(history.value));
      }
    });
    onMounted(() => {
      history.value = JSON.parse(localStorage.getItem("history")) ?? [];
      if (props.delHistory === true) {
        history.value.length = 0;
      }
    });
    return { player, boards, winner, addMove, history, tie };
  },
};
</script>

<style>
.container {
  display: flex;
  flex-wrap: wrap;
  margin: 0 auto;
}
.container .board {
  display: flex;
  flex-direction: column;
  width: 40%;
  align-items: center;
  margin: auto;
  user-select: none;
}
.container .c-right {
  width: 60%;
  margin-left: -500px;
  animation: 1s ease-out 0s 1 slideInFromRight;
}
.row {
  display: flex;
}
.row:first-of-type {
  border-bottom: 1px solid #293240;
}
.row:last-of-type {
  border-top: 1px solid #293240;
}
.cell {
  width: 200px;
  height: 200px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  font-size: 7rem;
  font-family: "Permanent Marker", sans-serif;
}
.column {
  border-right: 1px solid #293240;
}
.cell:last-of-type {
  border-right: 0px;
}
.turn {
  margin: 1rem 1rem 3rem 1rem;
}
.wins {
  margin: 1rem 1rem 2rem 1rem;
  font-size: 3rem;
  text-shadow: 0 0.1em 20px rgba(0, 0, 0, 1), 0.05em -0.03em 0 rgba(0, 0, 0, 1),
    0.05em 0.005em 0 rgba(0, 0, 0, 1), 0em 0.08em 0 rgba(0, 0, 0, 1),
    0.05em 0.08em 0 rgba(0, 0, 0, 1), 0px -0.03em 0 rgba(0, 0, 0, 1),
    -0.03em -0.03em 0 rgba(0, 0, 0, 1), -0.03em 0.08em 0 rgba(0, 0, 0, 1),
    -0.03em 0 0 rgba(0, 0, 0, 1);
}
.wins span {
  transform: scale(0.9);
  display: inline-block;
}
.wins span:first-child {
  animation: bop 1s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards infinite
    alternate;
}
.wins span:last-child {
  animation: bopB 1s 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards
    infinite alternate;
}
.c-right h2 {
  font-size: 3rem;
}
.history {
  font-size: 1.2rem;
}
.x-color {
  color: #f76d6e;
}

.o-color {
  color: #3aaea9;
}
.tie {
  color: white;
}
@media screen and (max-width: 1630px) {
  .container .c-right {
    width: 40%;
    margin-left: -500px;
  }
  .cell {
    width: 200px;
    height: 200px;
  }
  .btn {
    margin-top: 2rem;
  }
}
@media screen and (max-width: 1300px) {
  .container .c-right {
    width: 100%;
    margin: 3rem auto;
    animation: none;
  }
  .cell {
    width: 130px;
    height: 130px;
    font-size: 5rem;
  }
  .btn {
    margin-top: 2rem;
  }
}
@media screen and (max-width: 400px) {
  .cell {
    width: 80px;
    height: 80px;
  }
  .wins {
    margin: 1rem 1rem 2rem 1rem;
    font-size: 3rem;
    text-shadow: none;
  }
}
</style>
