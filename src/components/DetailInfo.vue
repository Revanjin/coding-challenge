<template>
  <div class="detail__title" v-if="planetInfo?.name">
    Welcome to {{ planetInfo.name }}
  </div>
  <div class="detail__title" v-else>We have no information about the name.</div>

  <div class="detail__item" v-if="planetInfo?.climate">
    You will find {{ planetInfo.climate }} climate
  </div>
  <div class="detail__item" v-else>We have no data about the climate</div>

  <div class="detail__item" v-if="planetInfo?.terrain">
    On this <i class="fas fa-globe-europe"></i> we have
    {{ planetInfo.terrain }}
  </div>
  <div class="detail__item" v-else>
    There is no information about the terrain
  </div>

  <div class="detail__item" v-if="planetInfo?.population > 0">
    <i class="fas fa-users"></i>
    {{ numberWithCommas(planetInfo.population) }} beings live on this planet
  </div>
  <div class="detail__item" v-else>Missing Data: Population Count</div>

  <router-link class="button" to="/">Back to the compendium </router-link>
</template>

<script>
export default {
  name: "DetailInfo",
  props: {
    planetInfo: Object,
  },
  methods: {
    numberWithCommas(x) {
      return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
    },
  },
};
</script>
