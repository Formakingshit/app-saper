<template>
  <div>
    <app-header></app-header>
    <app-field></app-field>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Field from "./components/Field.vue";

export default {
  components: {
    appHeader: Header,
    appField: Field
  }
};
</script>

<style scoped>
</style>
