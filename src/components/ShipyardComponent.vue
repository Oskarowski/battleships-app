<template lang="">
  <div class="shipyard">
    <IndividalShip
      v-bind:key="individualShip.id"
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

  methods: {
    fieldClicked: function (fieldElement) {
      if (fieldElement.isMarked == true) {
        this.markedFields.push(fieldElement);
      } else {
        var whereField = this.markedFields.findIndex((element) => {
          return (
            element.fieldRow == fieldElement.fieldRow &&
            element.fieldColumn == fieldElement.fieldColumn
          );
        });
        this.markedFields.splice(whereField, 1);
      }
    },

    findCurrentShip: function () {
      return this.allShips.find((s) => {
        return s.state == "currentlyPickedShip";
      });
    },
    setNextShipToPick: function () {
      var eFreeShip = this.allShips.find((s) => s.state == "free");

      if (!eFreeShip) {
        return false;
      }
      eFreeShip.state = "currentlyPickedShip";
      return true;
    },

    validateShip: function (shipArray) {
      var currentShip = this.findCurrentShip();
      if (!currentShip) {
        return this.$emit("all-ships-picked", this.allShips);
      }
      if (currentShip.length == shipArray.length) {
        console.log(shipArray);
        this.$emit("ship-picked", shipArray);
        currentShip.state = "alreadyPickedShip";
        currentShip.pickedNodes = shipArray;
        this.markedFields = [];
        this.setNextShipToPick();
      }
    },
  },

  watch: {
    markedFields: {
      deep: true,
      handler: function (allFields) {
        var nodes = [];

        var findField = (fieldsToSearch, currentField, direction) => {
          var checkColumn = currentField.fieldColumn + 0;
          if (direction == "L") {
            checkColumn = checkColumn - 1;
          }
          if (direction == "R") {
            checkColumn = checkColumn + 1;
          }

          var fieldRow = currentField.fieldRow + 0;
          if (direction == "D") {
            fieldRow = fieldRow - 1;
          }
          if (direction == "U") {
            fieldRow = fieldRow + 1;
          }

          var found = fieldsToSearch.find((e) => {
            return e.fieldColumn == checkColumn && e.fieldRow == fieldRow;
          });

          return found;
        };

        var checkOther = (field, directions) => {
          var found = nodes.find(
            (e) =>
              e.fieldColumn == field.fieldColumn && e.fieldRow == field.fieldRow
          );
          if (found) return [];
          nodes.push(field);

          directions.forEach((d) => {
            var result = findField(allFields, field, d);

            if (result) {
              if (d == "U" || d == "D") {
                return [...checkOther(result, ["U", "D"], result)];
              }
              if (d == "L" || d == "R") {
                return [...checkOther(result, ["L", "R"], result)];
              } else {
                return [];
              }
            }
          });

          return [];
        };

        if (allFields.length > 0) {
          checkOther(allFields[0], ["U", "D", "L", "R"]);
        }
        this.validateShip(nodes);
      },
    },
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
