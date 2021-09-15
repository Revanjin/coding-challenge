<template>
  <div
    class="button"
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
    filter: Object,
    index: Number,
  },
  emits: ["filter-cards", "set-active"],
};
</script>