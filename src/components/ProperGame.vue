<template>
  <div class="game-container">
    <BattlefieldMap></BattlefieldMap>
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

  display: flex;
  justify-content: center;
}
</style>
