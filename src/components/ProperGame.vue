<template>
  <div class="game-container">
    <hr />
    <div class="proper-board">
      <BattlefieldMap></BattlefieldMap>
    </div>
    <hr />
  </div>
</template>

<script>
import BattlefieldMap from "./BattlefieldMap.vue";

export default {
  name: "ProperGame",
  components: {
    BattlefieldMap,
  },
};
</script>

<style scoped>
.game-container {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  padding: 0;
  margin: 0;
}

.proper-board {
  display: flex;
  justify-content: center;
  align-content: center;
  align-items: center;
}

hr {
  display: block;
  margin-top: 0.3em;
  margin-bottom: 0.3em;
  margin-left: auto;
  margin-right: auto;
  border-style: inset;
  border-width: 2px;
}
</style>
