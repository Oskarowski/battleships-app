<template>
  <div class="container">
    <h1>by: Oskar Kacprzak</h1>
    <h2 onclick="window.open('https://fundacja.addictive.pl/')">
      chaired by: Fundacja Grupy Addictive
    </h2>
    <button
      class="git-btn"
      onclick="window.open('https://github.com/Oskarowski')"
    ></button>

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
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.btn {
  padding: 2rem;
  background-color: rgb(115, 147, 179);
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

h2:hover {
  opacity: 0.9;
  cursor: pointer;
  font-size: 1.6em;
}

.git-btn {
  background-image: url("../assets/GithubLogo.png");
  background-size: cover;
  background-position: center;
  color: transparent;
  cursor: pointer;
  padding: 0;
  margin: 1rem;
  width: 18em;
  height: 18em;
  border-radius: 50%;
  border: 0px solid rgb(115, 147, 179);
}
.git-btn:hover {
  opacity: 0.9;
  cursor: pointer;
}
</style>
