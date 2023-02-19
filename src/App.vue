<template>
  <HomeMenu
    v-if="isMomeMenuState"
    @play-btn-clicked="playBtnClicked"
    @author-btn-clicked="authorBtnClicked"
  ></HomeMenu>
  <AboutAuthor
    v-if="isAboutAuthorState"
    @go-back-btn-clicked="goBackBtnClicked"
  ></AboutAuthor>
  <ProperGame v-if="isGameState"> </ProperGame>
</template>

<script>
import HomeMenu from "@/components/HomeMenu.vue";
import ProperGame from "@/components/ProperGame.vue";
import AboutAuthor from "@/components/AboutAuthor.vue";

export default {
  name: "App",
  data: function () {
    return {
      isMomeMenuState: false,
      isGameState: true,
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
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  padding: 0;
  margin: 0;

  width: 100%;
  height: 100%;
}
</style>
