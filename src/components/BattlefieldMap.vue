<template>
  <div class="battlefield-map">
    <SingleField
      @field-clicked="fieldClicked"
      :key="field"
      v-for="field in allFieldsCollection"
      :fieldElement="field"
      :isFieldMiss="field.isFieldMiss"
      :isFieldHit="field.isFieldHit"
      :isFieldSunk="field.isFieldSunk"
      :block="blockPicking"
    ></SingleField>
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
  },

  mounted() {
    this.generateAllMapFields();
  },

  methods: {
    fieldClicked: function (fieldElement) {
      this.$emit("field-clicked", fieldElement);
    },
    generateAllMapFields: function () {
      for (var i = 0; i < 10; i++) {
        for (var j = 0; j < 10; j++) {
          this.allFieldsCollection.push({
            fieldRow: i,
            fieldColumn: j,
            frontID: `${i}, ${j}`,
            isFieldMiss: false,
            isFieldHit: false,
            isFieldSunk: false,
            isFieldBlocked: false,
          });
        }
      }
    },
  },
};
</script>

<style>
.battlefield-map {
  height: 100%;
  width: 100%;

  max-height: max-content;
  max-width: max-content;
  min-height: min-content;
  min-width: min-content;

  margin-left: 1vw;
  margin-right: 1vw;

  border: 0.1rem solid darkblue;
  border-radius: 0.2rem;

  display: grid;
  grid-template-rows: repeat(10, auto);
  grid-template-columns: repeat(10, auto);
}
</style>
