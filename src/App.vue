<template>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link
    href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap"
    rel="stylesheet"
  />
  <HomeMenu
    v-if="isMomeMenuState"
    @play-btn-clicked="playBtnClicked"
    @author-btn-clicked="authorBtnClicked"
  ></HomeMenu>
  <AboutAuthor
    v-if="isAboutAuthorState"
    @go-back-btn-clicked="goBackBtnClicked"
  ></AboutAuthor>
  <ProperGame @back-to-menu="backToMenuClicked()" v-if="isGameState">
  </ProperGame>
</template>

<script>
import HomeMenu from "@/components/HomeMenu.vue";
import ProperGame from "@/components/ProperGame.vue";
import AboutAuthor from "@/components/AboutAuthor.vue";

export default {
  name: "App",
  data: function () {
    return {
      isMomeMenuState: true,
      isGameState: false,
      isAboutAuthorState: false,
    };
  },
  components: {
    HomeMenu,
    ProperGame,
    AboutAuthor,
  },

  methods: {
    playBtnClicked: function () {
      // console.log("APP: playBtnClicked");
      this.isMomeMenuState = false;
      this.isAboutAuthorState = false;
      this.isGameState = true;
    },
    authorBtnClicked: function () {
      // console.log("APP: authorBtnClicked");
      this.isMomeMenuState = false;
      this.isAboutAuthorState = true;
      this.isGameState = false;
    },
    goBackBtnClicked: function () {
      // console.log("APP: goBackBtnClicked");
      this.isMomeMenuState = true;
      this.isAboutAuthorState = false;
      this.isGameState = false;
    },

    backToMenuClicked: function () {
      console.log("App: backToMenuClicked");

      this.isMomeMenuState = true;
      this.isGameState = false;
      this.isAboutAuthorState = false;
    },
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  font-family: "Press Start 2P", cursive;
}
button {
  font-family: "Press Start 2P", cursive;
}
#app {
  font-family: "Press Start 2P";
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  display: flex;
  justify-content: center;
  align-items: center;
  align-content: center;

  padding: 0;
  margin: 0;

  width: 100%;
  height: 100%;
}
</style>
