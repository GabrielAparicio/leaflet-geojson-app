<template>
  <b-button variant="info" @click="showData">Save</b-button>
</template>

<script>
export default {
  name: "SaveButton",
  methods: {
    showData() {
      console.log("This button is goint to show the list of markers");
    }
  },
  computed: {}
};
</script>

<style></style>
