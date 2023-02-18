<template>
  <div class="home-menu-container">
    <h1>Battleship The Game</h1>
    <div class="btns-container">
      <button @click="playBtnClicked" class="btn">Play</button>
      <button @click="authorBtnClicked" class="btn">Author</button>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    playBtnClicked: function () {
      this.$emit("play-btn-clicked");
    },

    authorBtnClicked: function () {
      this.$emit("author-btn-clicked");
    },
  },
};
</script>

<style scoped>
.home-menu-container {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  cursor: default;
  display: flex;
  justify-content: center;
  flex-direction: column;
}
.btn {
  padding: 2rem;
  background-color: green;
  font-size: 2em;
  border-radius: 1em;
  border: 0;
  margin: 1rem;
  color: white;
}
.btns-container {
  display: flex;
  flex-direction: column;
  align-content: center;
  justify-content: space-between;
  flex-wrap: wrap;
}
.btn:hover {
  opacity: 0.9;
  cursor: pointer;
}
</style>
