<template>
  <div class="home-menu-container">
    <h1>Battleships The Game</h1>
    <h3>by: Oskar Kacprzak</h3>
    <a href="https://github.com/Oskarowski" target="_blank" rel="noopener"
      >GitHub</a
    >
    <div>
      <button @click="playBtnClicked" class="btn">PLAY</button>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    playBtnClicked: function () {
      this.$emit("play-btn-clicked");
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

  display: flexbox;
  cursor: default;
}

.btn {
  padding: 1rem;
  background-color: green;
  font-size: 1rem;
  border-radius: 2em;
  border: 0;
  margin: 1rem;
  color: white;
}
.btn:hover {
  opacity: 0.9;
  cursor: pointer;
}
</style>
