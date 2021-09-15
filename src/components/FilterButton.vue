<template>
  <div
    class="filter-button"
    @click="
      $emit('filter-cards', filter.url);
      $emit('set-active', index);
    "
  >
    {{ filter.title }}
  </div>
</template>

<script>
export default {
  name: "FilterButton",
  props: {
    filter: Array,
    index: Number,
  },
};
</script>