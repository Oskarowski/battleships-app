<template>
  <div class="container">
    <h1>About Author</h1>
    <h3>by: Oskar Kacprzak</h3>
    <a href="https://github.com/Oskarowski" target="_blank" rel="noopener"
      >GitHub</a
    >
    <button @click="goBackBtnClicked" class="btn">Go Back</button>
  </div>
</template>

<script>
export default {
  methods: {
    goBackBtnClicked: function () {
      this.$emit("go-back-btn-clicked");
    },
  },
};
</script>

<style scoped>
.container {
  padding: 0;
  margin: 0;
  width: 100%;
  height: 100%;
  backdrop-filter: blur(30px);
  background-color: azure;
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
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
.btn:hover {
  opacity: 0.9;
  cursor: pointer;
}
</style>
