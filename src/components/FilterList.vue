<template>
  <div class="filter-button filter__toggle" @click="$emit('toggle-filter')">
    <i class="fas fa-filter"></i> Filter
  </div>
  <div class="filter__container">
    <div class="filter__scroll-container">
      <div class="filter__overflow-container">
        <div v-for="(filter, index) in filters" :key="index">
          <FilterButton
            :class="{ 'button--active': currentIndex === index }"
            :filter="filter"
            :index="index"
            @filter-cards="$emit('filter-cards', filter.url)"
            @set-active="setActive"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import FilterButton from "./FilterButton.vue";

export default {
  name: "FilterList",
  data() {
    return { currentIndex: 0 };
  },
  components: {
    FilterButton,
  },
  emits: ["filter-cards", "toggle-filter"],
  props: {
    filters: Array,
  },
  methods: {
    setActive(index) {
      this.currentIndex = index;
    },
  },
};
</script>