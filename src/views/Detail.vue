<template>
  <LoadingSpinner v-if="isLoading" :isLoading="isLoading" />
  <div class="detail__container">
    <div v-if="!isLoading" class="detail" @click="toggleInformation">
      <div class="detail__title" v-if="planetInfo?.name">
        Welcome to {{ planetInfo.name }}
      </div>
      <div class="detail__title" v-else>
        We have no information about the name.
      </div>

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
    </div>
  </div>
  <div v-if="!isLoading" class="planet__container">
    <div class="tooltip">
      <div class="tooltip__container">Click the planet</div>
    </div>
    <div
      class="planet"
      @click="toggleInformation"
      v-bind:style="{
        animation:
          'spin-planet ' +
          (planetInfo?.rotation_period > 0
            ? planetInfo?.rotation_period + 's'
            : '24s') +
          ' linear infinite',
        height:
          planetInfo?.diameter > 1000
            ? planetInfo.diameter / 30 + 'px'
            : '350px',
        width:
          planetInfo?.diameter > 1000
            ? planetInfo.diameter / 30 + 'px'
            : '350px',
      }"
    ></div>
  </div>
</template>

<script>
import LoadingSpinner from "../components/LoadingSpinner.vue";
export default {
  name: "detail",
  components: {
    LoadingSpinner,
  },
  data() {
    return {
      planetUrl: "https://swapi.dev/api/planets/",
      planetInfo: [],
      planets: [],
      isLoading: true,
    };
  },
  created() {
    var url = window.location.hash;
    var index = url.lastIndexOf("/");
    var query = url.substr(index + 1);
    console.log(query);

    this.fetchPlanets(this.planetUrl, query);
  },
  methods: {
    fetchPlanets(url, query) {
      fetch(url)
        .then((response) => {
          if (!response.ok) {
            throw Error("error while fetching", url, response);
          }

          return response.json();
        })
        .then((data) => {
          var planetData = data.results.find((planet) => {
            return planet.name == query;
          });

          if (data.next != null && planetData == undefined) {
            this.fetchPlanets(data.next, query);
          } else {
            this.planetInfo = planetData;
            this.isLoading = false;
          }
        });
    },
    numberWithCommas(x) {
      return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
    },
    toggleInformation() {
      var scrollContainer = document.querySelector(".detail");
      var container = document.querySelector(".detail__container");
      var tooltip = document.querySelector(".tooltip");

      if (container.offsetHeight > 0) {
        scrollContainer.classList.remove("detail--open");
        tooltip.style.opacity = 1;
        setTimeout(() => {
          container.style.height = 0;
        }, 500);
      } else {
        container.style.height = scrollContainer.scrollHeight + 16 + "px";
        scrollContainer.classList.add("detail--open");
        tooltip.style.opacity = 0;
      }
    },
  },
};
</script>