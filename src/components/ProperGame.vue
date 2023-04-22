<template>
  <div class="game-container">
    <div v-if="isGameOn">
      <hr />
      <label> {{ playerName }} VS {{ opponentName }} </label>
      <!-- <p>theGameIsOn: {{ theGameIsOn }}</p> -->
      <hr />
      <div class="all-fields-board">
        <BattlefieldMap
          v-if="displayMainBoard"
          ref="proper_battlefield"
          @field-clicked="fieldClicked"
          :textToDisplayAbove="'The Battlefield:'"
          :blockPicking="blockPicking"
        ></BattlefieldMap>
      </div>
      <hr />
      <ShipyardComponent
        ref="shipyardComponent"
        @ship-picked="individualShipPicked($event)"
        v-on:all-ships-picked="allShipsPicked($event)"
      ></ShipyardComponent>
      <hr />
      <div class="all-my-picked-fields">
        <BattlefieldMap
          v-if="theGameIsOn"
          ref="referal_battlefield"
          :blockPicking="true"
          :drawFieldsPickedByPlayer="myPickedFields"
          :textToDisplayAbove="'Your Placement:'"
        ></BattlefieldMap>
      </div>
    </div>
    <div v-if="showEndGameScreen">
      <EndGameComponent @back-to-menu="backToMenuClicked()"> </EndGameComponent>
    </div>
  </div>
</template>

<script>
import BattlefieldMap from "./BattlefieldMap.vue";
import ShipyardComponent from "./ShipyardComponent.vue";
import EndGameComponent from "./EndGameComponent.vue";

import Swal from "sweetalert2";

export default {
  emits: ["back-to-menu"],
  name: "ProperGame",
  components: {
    BattlefieldMap,
    ShipyardComponent,
    EndGameComponent,
  },

  data: function () {
    return {
      playerName: null,
      opponentName: null,
      myPickedFields: [],
      blockPicking: false,
      displayMainBoard: true,
      theGameIsOn: false,
      playerLost: false,
      playerWon: false,
      isGameOn: true,
      showEndGameScreen: false,
    };
  },

  props: {
    opponentNameApp: undefined,
    mySlotIndex: undefined,
    socket: undefined,
  },

  mounted: function () {
    this.socket.on("opponentIsReady", () => {
      Swal.fire({
        toast: true,
        position: "top",
        title: `${this.opponentName} placed ships`,
      });
    });

    this.getPlayerName();

    this.socket.on("blockClickingOnBoard", () => {
      this.blockPicking = true;
    });
    this.socket.on("yourTurn", () => {
      this.blockPicking = false;
    });

    this.socket.on("theGameIsOn", () => {
      this.theGameIsOn = true;
      this.displayMainBoard = false;
      this.$nextTick(() => {
        this.theGameIsOn = true;
        this.displayMainBoard = true;
      });
    });

    this.socket.on("shotHit", (data) => {
      var fieldElement = data.fieldElement;
      Swal.fire({
        toast: true,
        title: "Ship hit 🔥",
        showConfirmButton: false,
        position: "top",
        timer: 1000,
      });
      if (data.shotHitBy === this.mySlotIndex) {
        this.$nextTick(() => {
          this.$refs.proper_battlefield.fieldHitByPlayer(fieldElement);
        }).catch(function () {});
      } else if (data.shotHitBy !== undefined) {
        this.$nextTick(() => {
          this.$refs.referal_battlefield.fieldHitByPlayer(fieldElement);
        }).catch(function () {});
      }
    });

    this.socket.on("shotMissed", ({ fieldElement, missedBy }) => {
      Swal.fire({
        toast: true,
        title: "Missed",
        showConfirmButton: false,
        position: "top",
        timer: 1000,
      });

      if (missedBy === this.mySlotIndex) {
        this.$nextTick(() => {
          this.$refs.proper_battlefield.shotMissedByPlayer(fieldElement);
        }).catch(function () {});
      } else if (missedBy !== undefined) {
        this.$nextTick(() => {
          this.$refs.referal_battlefield.shotMissedByPlayer(fieldElement);
        }).catch(function () {});
      }
    });

    this.socket.on("hitAndSunk", ({ hitBy, ship }) => {
      //TODO: add heare switch to get a ship full name not just ID
      var textToDisplayinAlert;

      ship.pickedNodes.forEach((node) => {
        if (hitBy === this.mySlotIndex) {
          textToDisplayinAlert = `${this.playerName} sunk ${this.opponentName} ${ship.id}`;
          this.$nextTick(() => {
            this.$refs.proper_battlefield.shipSunkByPlayer(node);
          }).catch(function () {});
        } else if (hitBy !== undefined) {
          textToDisplayinAlert = `${this.opponentName} sunk ${this.playerName} ${ship.id}`;
          this.$nextTick(() => {
            this.$refs.referal_battlefield.shipSunkByPlayer(ship.id);
          }).catch(function () {});
        }
      });

      if (hitBy === this.mySlotIndex) {
        this.$nextTick(() => {
          this.$refs.shipyardComponent.markThatOpponentsShipSunk(ship);
        }).catch(function () {});
      }

      Swal.fire({
        title: "Ship sunk",
        text: textToDisplayinAlert,
        showConfirmButton: false,
        timer: 1500,
      });
    });

    this.socket.on("theGameIsOver", () => {
      Swal.fire({
        title: "THE GAME IS OVER",
        showConfirmButton: true,
        timer: 1500,
      });
      this.blockPicking = true;
      this.$nextTick(() => {
        this.isGameOn = false;
        this.theGameIsOn = false;
        this.displayMainBoard = false;
        this.showEndGameScreen = true;
      });

      this.socket.emit("theGameIsOver", true);
    });

    this.socket.on("youWin", () => {
      this.playerWon = true;
      Swal.fire({
        title: "YOU WON 🔥",
        position: "top",
        showConfirmButton: false,
        timer: 2500,
      });
    });

    this.socket.on("youLoose", () => {
      this.playerLost = true;
      Swal.fire({
        title: "YOU LOST 😵",
        position: "top",
        showConfirmButton: false,
        timer: 2500,
      });
    });

    this.socket.on("connect", function () {
      // Swal.fire({
      //   toast: true,
      //   showConfirmButton: false,
      //   text: "Connection established",
      //   timer: 1000,
      // });
    });

    this.socket.on("disconnect", function () {});
  },

  methods: {
    fieldClicked: function (fieldElement) {
      if (this.theGameIsOn) {
        this.socket.emit("shotFired", { fieldElement: fieldElement });
      } else {
        this.socket.emit("fieldClicked", fieldElement);
        this.$refs.shipyardComponent.fieldClicked(fieldElement);
      }
    },
    allShipsPicked: function (shipsPickedData) {
      shipsPickedData.forEach((ship) => {
        this.myPickedFields.push(...ship.pickedNodes);
      });
      this.blockPicking = true;
      this.socket.emit("allShipsPicked", shipsPickedData);
    },

    individualShipPicked: function (shipArray) {
      this.$nextTick(() => {
        this.$refs.proper_battlefield.blockIndividualNodes(shipArray);
      }).catch(function () {});
    },

    getPlayerName: function () {
      this.blockPicking = true;
      Swal.fire({
        toast: true,
        input: "text",
        text: "Enter Your nick",
        inputPlaceholder: "Here put your nick",
        position: "top",
        showCloseButton: false,
        showCancelButton: false,
      }).then((result) => {
        if (result.value) {
          this.playerName = result.value;
          this.blockPicking = false;
          this.socket.emit("setPlayerName", this.playerName);
          if (this.opponentNameApp !== undefined) {
            this.refreshPlayersNamesOnTop(this.opponentNameApp);
          }
        } else {
          this.getPlayerName();
        }
      });
    },

    getMainBoardRef: function () {},
    getRefBoardRef: function () {
      this.$nextTick(() => {
        return this.$refs.referal_battlefield;
      });
    },

    backToMenuClicked: function () {
      this.$emit("back-to-menu");
    },

    refreshPlayersNamesOnTop: function (recivedName) {
      this.opponentName = recivedName;
      this.$nextTick(() => {
        this.opponentName = recivedName;
      });
    },
  },
};
</script>

<style scoped>
.game-container {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 0;
  margin: 0;
}

.all-my-picked-fields {
  width: 25vh;
  margin: auto;
  display: flex;
  justify-content: center;
  align-content: center;
  align-items: center;
}
.all-fields-board {
  display: flex;
  justify-content: center;
  align-content: center;
  align-items: center;
}

hr {
  display: block;
  margin-top: 0.4vh;
  margin-bottom: 0.4vh;
  margin-left: auto;
  margin-right: auto;
  border-style: inset;
  border-width: 2px;
}
</style>
