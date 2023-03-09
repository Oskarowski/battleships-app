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

  mounted() {
    // console.log(this.drawFieldsPickedByPlayer);

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
      // console.log(fieldElement);
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
  },
};
</script>

<style>
.battlefield-map {
  height: 40vh;

  margin-left: 1vw;
  margin-right: 1vw;

  border: 0.1rem solid darkblue;
  border-radius: 0.2rem;

  display: grid;
  grid-template-rows: repeat(10, auto);
  grid-template-columns: repeat(10, auto);
}
</style>
