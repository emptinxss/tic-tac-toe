<template>
  <div class="form">
    <form @submit.prevent="getSetup(player, opponent, clearHistory)">
      <h2>SETUP</h2>
      <label> Player name: </label>
      <input type="text" v-model="player" required />
      <label> Opponent name: </label>
      <input type="text" v-model="opponent" required />

      <div>
        <label> Clear history: </label>
        <input type="checkbox" v-model="clearHistory" />
      </div>

      <div class="start-btn">
        <button type="submit">Start game</button>
      </div>
    </form>
  </div>
</template>

<script>
import { ref } from "@vue/reactivity";
export default {
  props: ["getSetup"],
  setup() {
    const player = ref("Luke");
    const opponent = ref("Paul");
    const clearHistory = ref(false);

    return { player, opponent, clearHistory };
  },
};
</script>

<style scoped>
.form {
  max-width: 440px;
  margin: 30px auto;
  background-color: rgba(41, 50, 64, 0.1);
  text-align: left;
  border-radius: 10px;
  padding: 30px;
  font-size: 1.2rem;
  font-family: "Luckiest Guy", sans-serif;
  animation: 1s ease-out 0s 1 slideInFromLeft;
}
form h2 {
  text-align: center;
  font-weight: 700;
  font-size: 3rem;
  margin: 1rem;
  line-height: 0.8;
  letter-spacing: 6px;
}
label {
  display: inline-block;
  margin: 25px 0 15px;
  font-size: 0.6em;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}

label:first-of-type {
  margin-right: 1.2rem;
}
input {
  padding: 10px;
  margin: 20px;
  border: none;
  border-bottom: 1px solid #293240;
  background: none;
  text-align: center;
  font-family: "Luckiest Guy", sans-serif;
  font-size: 1rem;
}
input:focus {
  outline: none;
}
.start-btn {
  text-align: center;
}

@media screen and (max-width: 490px) {
  .form {
    max-width: 380px;
    text-align: center;
  }
}
</style>
