<template>
  <button
    :disabled="isFieldBlocked"
    v-on:click="fieldClicked()"
    :class="{
      'single-field': true,
      'field-marked': isMarked,
      'field-hit': isFieldHit,
      'field-sunk': isFieldSunk,
      'field-missed': isFieldMissed,
    }"
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
    isFieldMissed: null,
    isFieldHit: null,
    isFieldSunk: null,
    isFieldBlocked: null,
  },

  mounted() {
    if (this.fieldElement.isMarked) {
      this.isMarked = true;
    }
  },

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
.field-hit {
  background-color: rgb(148, 0, 0);
}

@media (hover: hover) and (pointer: fine) {
  button:hover {
    opacity: 0.9;
    background-color: bisque;
    border-color: bisque;
    transition: opacity 0.2s, background-color 0.2s;
  }
}
.field-missed {
  background-color: gray;
}

.field-sunk {
  background-color: black;
}
</style>
