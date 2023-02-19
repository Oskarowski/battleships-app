<template>
  <button
    :disabled="block"
    v-on:click="fieldClicked()"
    :class="{ 'single-field': true, 'field-marked': isMarked }"
  >
    {{ fieldElement.frontID }}
  </button>
</template>

<script>
import { v4 } from "uuid";
export default {
  data: function () {
    return {
      fieldID: v4(),
      isMarked: false,
    };
  },
  props: {
    fieldElement: null,
    isFieldMiss: null,
    isFieldHit: null,
    isFieldSunk: null,
    block: null,
  },

  mounted() {},

  methods: {
    fieldClicked: function () {
      this.isMarked = !this.isMarked;

      this.$emit("field-clicked", {
        isMarked: this.isMarked,
        fieldRow: this.fieldElement.fieldRow,
        fieldColumn: this.fieldElement.fieldColumn,
      });
    },
  },
};
</script>

<style scoped>
.single-field {
  aspect-ratio: 1 / 1;
  width: 100%;
  height: 100%;

  max-height: 100%;
  max-width: 100%;

  min-width: min-content;
  min-width: min-content;

  border: 1px solid black;
  box-sizing: border-box;
  text-align: center;
}
.field-marked {
  background-color: aquamarine;
}
@media (hover: hover) and (pointer: fine) {
  button:hover {
    opacity: 0.9;
    background-color: bisque;
    border-color: bisque;
    transition: opacity 0.2s, background-color 0.2s;
  }
}
</style>
