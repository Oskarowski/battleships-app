<template>
  <div class="end-game">
    <h1>END OF THE GAME</h1>
    <buttton class="btn" @click="backToMenu">BACK TO MENU</buttton>
  </div>
</template>

<script>
export default {
  emits: ["back-to-menu"],
  methods: {
    backToMenu: function () {
      this.$emit("back-to-menu");
    },
  },
};
</script>

<style scoped>
.end-game {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  top: 40vh;
}

.btn:hover {
  opacity: 0.9;
  cursor: pointer;
}
</style>
