<template>
  <div>
    {{ this.shipName }}
    <div
      :class="whichShipClassesToApply"
      v-bind:key="singleShipNode"
      v-for="singleShipNode in individualShip.length"
    ></div>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      shipName: this.individualShip.id,
    };
  },
  props: {
    individualShip: Object,
  },
  mounted: function () {},

  computed: {
    whichShipClassesToApply: function () {
      var shipClasses = "shipNode ";

      if (this.individualShip.state == "currentlyPickedShip") {
        shipClasses += "currentlyPickedShip";
      }
      if (this.individualShip.state == "alreadyPickedShip") {
        shipClasses += "alreadyPickedShip";
      }

      return shipClasses;
    },
  },
};
</script>

<style scoped>
.shipNode {
  background-color: aliceblue;
  border: 1px solid black;
  width: 1em;
  height: 1em;
  aspect-ratio: 1 / 1;
  display: inline-block;
}
.currentlyPickedShip {
  background-color: yellowgreen;
}
.alreadyPickedShip {
  background-color: green;
}
</style>
