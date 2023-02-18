<template>
  <div class="shipyard">
    <IndividalShip
      v-bind:key="individualShip"
      v-for="individualShip in allShips"
      :individualShip="individualShip"
    ></IndividalShip>
  </div>
</template>

<script>
import IndividalShip from "@/components/IndividualShip.vue";

export default {
  components: {
    IndividalShip: IndividalShip,
  },

  data: function () {
    return {
      markedFields: null,
      allShips: [
        { id: 1, shipName: "destroyer", length: 2, state: "free" },
        { id: 2, shipName: "submarine", length: 3, state: "free" },
        { id: 3, shipName: "carrier", length: 5, state: "free" },
      ],
    };
  },

  mounted: function () {
    this.allShips[0].state = "currentlyPickedShip";
    this.markedFields = [];
  },
};
</script>

<style>
.shipyard {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-around;
  max-width: 100%;
}
</style>
