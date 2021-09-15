<template>
  <div class="card__headline">{{ card.name }}</div>

  <div class="card__hair-color">
    <i class="fas fa-angle-right"></i> Hair color:
    {{ card.hair_color != "n/a" ? card.hair_color : unknown }}
  </div>

  <div class="card__eye-color">
    <i class="fas fa-angle-right"></i> Eye color:
    {{ card.eye_color != "n/a" ? card.eye_color : unknown }}
  </div>

  <div class="card__gender">
    <i class="fas fa-angle-right"></i> Gender: {{ card.gender }}
  </div>

  <router-link class="button" :to="'/detail/' + card.planet?.name">
    <i class="fas fa-angle-right"></i> Planet: {{ card.planet?.name }}
  </router-link>
</template>

<script>
export default {
  name: "Card",
  props: {
    card: Object,
  },
  data() {
    return {
      unknown: "unknown",
    };
  },
};
</script>