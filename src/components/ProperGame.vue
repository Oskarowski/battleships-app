<template>
  <div class="game-container">
    <label>{{ mySlotIndex }}</label>
    <hr />
    <div class="proper-board">
      <BattlefieldMap
        ref="battlefield"
        @field-clicked="fieldClicked"
        :blockPicking="blockPicking"
      ></BattlefieldMap>
    </div>
    <hr />
    <ShipyardComponent
      ref="shipyardComponent"
      @all-ships-picked="allShipsPicked($event)"
    ></ShipyardComponent>
    <hr />
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
      myPickedFields: null,
      blockPicking: false,
    };
  },

  mounted: function () {
    this.socket = io("192.168.1.123:8082");
    this.socket.on("yourID", (id) => (this.mySlotIndex = id));
    this.socket.on("playerReady", function () {
      Swal.fire({ toast: true, position: "top-end", title: "Player ready" });
    });
  },

  methods: {
    fieldClicked: function (fieldElement) {
      // console.log("PG: fieldClicked: ", fieldElement);
      this.$refs.shipyardComponent.fieldClicked(fieldElement);
    },
    allShipsPicked: function (shipsPickedData) {
      console.log("PG: allShipsArray: ", shipsPickedData);
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

.proper-board {
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
