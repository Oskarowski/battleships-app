<template>
  <div class="battlefield-map">
    <SingleField
      :key="field"
      v-for="field in allFieldsCollection"
      :fieldParams="field"
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

  mounted() {
    this.generateAllMapFields();
  },

  methods: {
    generateAllMapFields: function () {
      for (var i = 0; i < 100; i++) {
        this.allFieldsCollection.push({
          frontID: i,
        });
      }
    },
  },
};
</script>

<style>
.battlefield-map {
  height: 600px;
  width: 600px;
  display: grid;
  grid-template-rows: repeat(10, auto);
  grid-template-columns: repeat(10, auto);
}
</style>
