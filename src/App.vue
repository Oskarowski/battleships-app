<template>
  <HomeMenu
    v-if="isMomeMenuState"
    @play-btn-clicked="playBtnClicked"
  ></HomeMenu>
  <ProperGame v-if="isGameState"> </ProperGame>
</template>

<script>
import HomeMenu from "@/components/HomeMenu.vue";
import ProperGame from "@/components/ProperGame.vue";

export default {
  name: "App",
  data: function () {
    return {
      isMomeMenuState: true,
      isGameState: false,
    };
  },
  components: {
    HomeMenu,
    ProperGame,
  },

  methods: {
    playBtnClicked: function () {
      console.log("playBtnClicked");
      this.isMomeMenuState = false;
      this.isGameState = true;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  padding: 0;
  margin: 0;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
