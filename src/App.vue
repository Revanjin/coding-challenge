<template>
  <span id="stars"></span>
  <span id="stars2"></span>
  <span id="stars3"></span>
  <Header title="Star Wars Compendium" />
  <router-view />
</template>

<script>
import Header from "./components/Header.vue";

export default {
  name: "App",
  components: {
    Header,
  },
};
</script>
