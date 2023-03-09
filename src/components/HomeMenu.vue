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
h1 {
  font-family: "Press Start 2P";
  font-size: 4vw;
}
.home-menu-container {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  margin-top: 5vh;

  cursor: default;
  display: flex;
  justify-content: center;
  flex-direction: column;
}
.btn {
  padding: 4%;
  background-color: green;
  font-size: 3vw;
  border-radius: 1em;
  border: 0;
  margin: 0.5em;
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
