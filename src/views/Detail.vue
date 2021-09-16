<template>
  <LoadingSpinner v-if="isLoading" :isLoading="isLoading" />
  <div class="detail__container" v-if="!isLoading">
    <div class="detail" @click="toggleInformation">
      <DetailInfo :planetInfo="planetInfo" />
    </div>
  </div>
  <Planet :isLoading="isLoading" @toggle-information="toggleInformation" />
</template>

<script>
import LoadingSpinner from "../components/LoadingSpinner.vue";
import Planet from "../components/Planet.vue";
import DetailInfo from "../components/DetailInfo.vue";

export default {
  name: "detail",
  components: {
    LoadingSpinner,
    Planet,
    DetailInfo,
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

    document.querySelector("body").style.overflow = "hidden";

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