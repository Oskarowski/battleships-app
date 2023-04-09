<template>
  <div>
    <p>{{ textToDisplayAbove }}</p>
    <div class="battlefield-map">
      <SingleField
        @field-clicked="fieldClicked"
        :key="field"
        v-for="field in allFieldsCollection"
        :fieldElement="field"
        :isFieldMissed="field.isFieldMissed"
        :isFieldHit="field.isFieldHit"
        :isFieldSunk="field.isFieldSunk"
        :isFieldBlocked="blockPicking"
        ref="map_fields"
      ></SingleField>
    </div>
  </div>
</template>

<script>
import SingleField from "./SingleField.vue";

export default {
  name: "BattlefieldMap",
  components: {
    SingleField: SingleField,
  },

  data: function () {
    return {
      allFieldsCollection: [],
    };
  },

  props: {
    blockPicking: null,
    drawFieldsPickedByPlayer: null,
    textToDisplayAbove: null,
  },

  mounted: function () {
    this.generateAllMapFields();
  },

  methods: {
    fieldClicked: function (fieldElement) {
      this.$emit("field-clicked", fieldElement);
    },

    generateAllMapFields: function () {
      for (var i = 0; i < 10; i++) {
        for (var j = 0; j < 10; j++) {
          var overridedField;
          if (this.drawFieldsPickedByPlayer) {
            overridedField = this.drawFieldsPickedByPlayer.find((element) => {
              return element.fieldRow == i && element.fieldColumn == j;
            });
          }

          this.allFieldsCollection.push({
            fieldRow: i,
            fieldColumn: j,
            isMarked: overridedField ? true : false,
            frontID: `${i}, ${j}`,
            isFieldMissed: false,
            isFieldHit: false,
            isFieldSunk: false,
          });
        }
      }
    },

    fieldHitByPlayer: function (fieldElement) {
      this.allFieldsCollection.find(
        (cell) =>
          cell.fieldRow == fieldElement.fieldRow &&
          cell.fieldColumn == fieldElement.fieldColumn
      ).isFieldHit = true;
    },

    shotMissedByPlayer: function (fieldElement) {
      this.allFieldsCollection.find(
        (cell) =>
          cell.fieldRow == fieldElement.fieldRow &&
          cell.fieldColumn == fieldElement.fieldColumn
      ).isFieldMissed = true;
    },

    shipSunkByPlayer: function (fieldElement) {
      this.allFieldsCollection.find(
        (cell) =>
          cell.fieldRow == fieldElement.fieldRow &&
          cell.fieldColumn == fieldElement.fieldColumn
      ).isFieldSunk = true;
    },

    blockIndividualNodes: function (pickedNodesData) {
      pickedNodesData.forEach((node) => {
        this.$nextTick(() => {
          this.$refs.map_fields
            .find(
              (cell) =>
                cell.fieldElement.fieldRow == node.fieldRow &&
                cell.fieldElement.fieldColumn == node.fieldColumn
            )
            .blockFieldFromPicking();
        }).catch(function () {});
      });
    },
  },
};
</script>

<style>
.battlefield-map {
  height: 38vh;

  margin-top: 0;
  margin-left: 1vw;
  margin-right: 1vw;

  border: 0.1rem solid darkblue;
  border-radius: 0.2rem;

  display: grid;
  grid-template-rows: repeat(10, auto);
  grid-template-columns: repeat(10, auto);
}

p {
  margin: none;
  padding: 0;
}
</style>
