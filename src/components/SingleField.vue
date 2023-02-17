<template>
  <button
    v-on:click="btnClicked"
    :class="{ 'single-field': true, 'field-marked': isFieldMarked }"
  >
    {{ fieldParams.frontID }}
  </button>
</template>

<script>
import { v4 } from "uuid";
export default {
  data: function () {
    return {
      fieldID: v4(),
      isFieldMarked: false,
    };
  },
  props: {
    fieldParams: null,
  },

  mounted() {},

  methods: {
    btnClicked: function () {
      console.log("btnClicked: ", this.fieldParams.frontID);
      this.isFieldMarked = !this.isFieldMarked;
      // this.$emit("fieldClicked", {
      //   frontID: this.frontID,
      // });
    },
  },
};
</script>

<style scoped>
.single-field {
  max-height: 100%;
  max-width: 100%;
  border: 1px solid black;
  margin: 1px;
  aspect-ratio: 1/1;
}
.field-marked {
  background-color: aquamarine;
}

button:hover {
  opacity: 0.9;
  background-color: bisque;
}
</style>
