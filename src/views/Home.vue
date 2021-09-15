<template>
  <LoadingSpinner v-if="isLoading" :isLoading="isLoading" />
  <div class="container">
    <FilterList
      v-if="this.cards.length"
      :filters="filter"
      @toggle-filter="toggleFilter"
      @filter-cards="filterCards"
    />
    <div class="card__container">
      <CardList :cards="cards" />
    </div>
  </div>
</template>

<script>
import CardList from "../components/CardList.vue";
import FilterList from "../components/FilterList.vue";
import LoadingSpinner from "../components/LoadingSpinner.vue";
import { API_URL } from "../config/API_URL";

export default {
  name: "Home",
  components: { CardList, FilterList, LoadingSpinner },
  data() {
    return {
      peopleUrl: API_URL.people,
      movieUrl: API_URL.movies,
      planetUrl: API_URL.planets,
      cards: [],
      movies: [],
      people: [],
      planets: [],
      filter: [{ title: "Reset Filter", url: null }],
      isLoading: true,
    };
  },
  created() {
    this.initiallySetupMoviesList(this.movieUrl);
    this.initiallySetupPlanetsList(this.planetUrl);
    this.initiallySetupPeopleList(this.peopleUrl);
  },
  methods: {
    initiallySetupMoviesList(url) {
      fetch(url)
        .then((response) => response.json())
        .then((data) => {
          this.movies = data.results;
        });
    },
    initiallySetupPlanetsList(url) {
      fetch(url)
        .then((response) => response.json())
        .then((data) => {
          data.results.forEach((planet) => {
            this.planets.push({ name: planet.name, url: planet.url });
          });

          if (data.next != null) {
            this.initiallySetupPlanetsList(data.next);
          }
        });
    },
    initiallySetupPeopleList(url) {
      fetch(url)
        .then((response) => {
          if (!response.ok) {
            throw Error("error while fetching", url, response);
          }

          return response.json();
        })
        .then((data) => {
          data.results.forEach((person) => {
            person.planet = this.planets.find(
              (planet) => person.homeworld == planet.url
            );

            this.people.push(person);
          });

          if (data.next != null) {
            this.initiallySetupPeopleList(data.next);
          } else {
            this.cards = this.people;

            this.setFilterList();
            this.isLoading = false;
          }
        });
    },
    toggleFilter() {
      var scrollContainer = document.querySelector(".filter__scroll-container");
      var container = document.querySelector(".filter__container");

      if (container.offsetHeight > 0) {
        container.style.height = 0;
        scrollContainer.classList.remove("filter__scroll-container--open");
      } else {
        container.style.height = scrollContainer.scrollHeight + "px";
        scrollContainer.classList.add("filter__scroll-container--open");
      }
    },
    filterCards(url) {
      if (url != null) {
        this.cards = this.people.filter((person) => {
          return person.films.includes(url) ? person : "";
        });
      } else {
        this.cards = this.people;
      }

      setTimeout(() => {
        this.toggleFilter();
      }, 300);
    },
    setFilterList() {
      this.movies.forEach((movie) => {
        this.filter.push({ title: movie.title, url: movie.url });
      });
    },
  },
};
</script>

<style lang="scss">
@import "../scss/main.scss";
</style>
