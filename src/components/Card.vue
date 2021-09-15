<template>
  <div class="card__headline">{{ card.name }}</div>

  <div class="card__hair-color">
    Hair color: {{ card.hair_color != "n/a" ? card.hair_color : unknown }}
  </div>

  <div class="card__eye-color">
    Eye color: {{ card.eye_color != "n/a" ? card.eye_color : unknown }}
  </div>

  <div class="card__gender">
    Gender:
    <i
      v-if="card.gender == 'male'"
      class="fas fa-mars card__icon card__icon--male"
    ></i>
    <i
      v-if="card.gender == 'female'"
      class="fas fa-venus card__icon card__icon--female"
    ></i>
    <i
      v-if="card.gender == 'n/a'"
      class="fas fa-robot card__icon card__icon--divers"
    ></i>
  </div>

  <div class="card__planet">
    Planet:
    <a :href="card.homeworld" alt="link to homeworld" class="card__link">
      {{ card.planetName?.name }}
    </a>
  </div>
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