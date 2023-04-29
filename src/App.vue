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
  <ProperGame
    v-if="isGameState"
    ref="rGameComponent"
    @back-to-menu="backToMenuClicked()"
    :mySlotIndex="mySlotIndex"
    :socket="socket"
    :opponentNameApp="opponentName"
  >
  </ProperGame>
</template>

<script>
import HomeMenu from "@/components/HomeMenu.vue";
import ProperGame from "@/components/ProperGame.vue";
import AboutAuthor from "@/components/AboutAuthor.vue";

import { io } from "socket.io-client";
import Swal from "sweetalert2";

export default {
  name: "App",
  data: function () {
    return {
      isMomeMenuState: true,
      isGameState: false,
      isAboutAuthorState: false,

      mySlotIndex: undefined,
      socket: undefined,
      opponentName: null,
    };
  },
  components: {
    HomeMenu,
    ProperGame,
    AboutAuthor,
  },

  mounted: function () {
    this.socket = io("http://192.168.1.115:8082/");

    this.socket.on("yourID", (id) => {
      this.mySlotIndex = id;
    });

    this.socket.on("setOpponentName", (recivedName) => {
      this.opponentName = recivedName;
      if (this.isGameState === true) {
        this.$refs.rGameComponent.refreshPlayersNamesOnTop(recivedName);
      }
    });
  },

  methods: {
    playBtnClicked: function () {
      if (this.socket.connected) {
        this.socket.emit(
          "playBtnClicked",
          "Checking for opponent in room",
          (isThereOpponentInRoom) => {
            if (isThereOpponentInRoom === true) {
              this.isMomeMenuState = false;
              this.isAboutAuthorState = false;
              this.isGameState = true;
            } else {
              Swal.fire({
                title: "No players available",
                text: "Sorry, there are no players available to play with.",
                icon: "warning",
              });
            }
          }
        );
      } else {
        console.log("mySlotIndex", this.mySlotIndex);
        Swal.fire({
          title: "Connecting...",
          html: "Please wait...",
          allowEscapeKey: false,
          allowOutsideClick: false,
          showConfirmButton: false,
          didOpen: () => {
            Swal.showLoading();
            if (this.socket.connected) {
              Swal.close();
              Swal({
                title: "Connected",
                icon: "success",
                buttons: false,
              });
            } else {
              Swal.fire({
                title: "Falied connection",
                icon: "error",
                allowEscapeKey: false,
                allowOutsideClick: false,
                showConfirmButton: false,
              });
              setTimeout(function () {
                Swal.close();
              }, 2000);
            }
          },
        });
      }
    },
    authorBtnClicked: function () {
      this.isMomeMenuState = false;
      this.isAboutAuthorState = true;
      this.isGameState = false;
    },
    goBackBtnClicked: function () {
      this.isMomeMenuState = true;
      this.isAboutAuthorState = false;
      this.isGameState = false;
    },

    backToMenuClicked: function () {
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

  /* overflow: hidden; //problem with end screan, not showing off when this is turned on */
}
</style>
