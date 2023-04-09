<template>
  <button
    :disabled="isFieldNotPickable"
    v-on:click="fieldClicked()"
    :class="{
      'single-field': true,
      'field-marked': isMarked,
      'field-hit': isFieldHit,
      'field-sunk': isFieldSunk,
      'field-missed': isFieldMissed,
    }"
  >
    <!-- {{ fieldElement.frontID }} -->
  </button>
</template>

<script>
import { v4 } from "uuid";
export default {
  data: function () {
    return {
      fieldID: v4(),
      isMarked: false,
      isFieldNotPickable: false,
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
    if (this.isFieldBlocked) this.isFieldNotPickable = true;
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

    blockFieldFromPicking: function () {
      this.isFieldNotPickable = true;
    },
  },

  watch: {
    isFieldBlocked(newValue) {
      if (newValue === true) this.isFieldNotPickable = true;
      else if (newValue === false) this.isFieldNotPickable = false;
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

  transition: background-color 0.7s ease;
}
.field-marked {
  background-color: rgb(0, 87, 114);
}
.field-hit {
  background-color: rgb(98, 0, 0);
}

.field-missed {
  background-color: rgb(159, 159, 159);
}

.field-sunk {
  background-color: rgb(0, 0, 56);
}
@media (hover: hover) and (pointer: fine) {
  button:not(:disabled):hover {
    opacity: 0.9;
    background-color: bisque;
    border-color: bisque;
    transition: opacity 0.2s, background-color 0.2s;
  }
}
</style>
