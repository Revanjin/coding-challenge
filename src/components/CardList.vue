<template>
  <div v-for="card in cards" class="card" :key="card.id">
    <Card :card="card" />
  </div>
</template>

<script>
import Card from "./Card.vue";

export default {
  name: "CardList",
  props: {
    cards: Array,
  },
  components: {
    Card,
  },
};
</script>