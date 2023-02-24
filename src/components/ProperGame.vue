<template>
  <div class="game-container">
    <label> {{ mySlotIndex }} </label>
    <p>theGameIsOn: {{ theGameIsOn }}</p>
    <hr />
    <div class="all-fields-board">
      <BattlefieldMap
        v-if="displayMainBoard"
        ref="proper_battlefield"
        @field-clicked="fieldClicked"
        :blockPicking="blockPicking"
      ></BattlefieldMap>
    </div>
    <hr />
    <ShipyardComponent
      ref="shipyardComponent"
      v-on:all-ships-picked="allShipsPicked($event)"
    ></ShipyardComponent>
    <hr />
    <div class="all-my-picked-fields">
      <BattlefieldMap
        v-if="theGameIsOn"
        ref="referal_battlefield"
        :blockPicking="true"
        :drawFieldsPickedByPlayer="myPickedFields"
      ></BattlefieldMap>
    </div>
  </div>
</template>

<script>
import BattlefieldMap from "./BattlefieldMap.vue";
import ShipyardComponent from "./ShipyardComponent.vue";
import { io } from "socket.io-client";

import Swal from "sweetalert2";

export default {
  name: "ProperGame",
  components: {
    BattlefieldMap,
    ShipyardComponent,
  },

  data: function () {
    return {
      playerName: "Oskar",
      socket: null,
      mySlotIndex: undefined,
      myPickedFields: [],
      blockPicking: false,
      displayMainBoard: true,
      theGameIsOn: false,
      playerLost: false,
      playerWon: false,
    };
  },

  mounted: function () {
    this.socket = io("http://192.168.1.123:8082");
    this.socket.on("yourID", (id) => {
      console.log(id);
      this.mySlotIndex = id;
    });
    this.socket.on("opponentIsReady", function () {
      Swal.fire({
        toast: true,
        position: "top-end",
        title: "Opponent already placed ships",
      });
    });

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
        position: "top-right",
        timer: 1000,
      });
      if (data.shotHitBy === this.mySlotIndex) {
        this.$refs.proper_battlefield.fieldHitByPlayer(fieldElement);
      } else if (data.shotHitBy !== undefined) {
        this.$refs.referal_battlefield.fieldHitByPlayer(fieldElement);
      }
    });

    this.socket.on("connect", function () {
      Swal.fire({ toast: true, text: "Connection established" });
    });

    this.socket.on("disconnect", function () {
      console.log("Disconnected from server");
    });
  },

  methods: {
    fieldClicked: function (fieldElement) {
      if (this.theGameIsOn) {
        this.socket.emit("shotFired", { fieldElement: fieldElement });
        console.log("Shot fired");
      } else {
        this.socket.emit("fieldClicked", fieldElement);
        this.$refs.shipyardComponent.fieldClicked(fieldElement);
      }
    },
    allShipsPicked: function (shipsPickedData) {
      shipsPickedData.forEach((ship) => {
        this.myPickedFields.push(...ship.pickedNodes);
      });
      this.socket.emit("allShipsPicked", shipsPickedData);
    },
  },
};
</script>

<style scoped>
.game-container {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  padding: 0;
  margin: 0;
}

.all-my-picked-fields {
  /* background-color: red; */
  width: 50%;
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
  margin-top: 0.3em;
  margin-bottom: 0.3em;
  margin-left: auto;
  margin-right: auto;
  border-style: inset;
  border-width: 2px;
}
</style>
