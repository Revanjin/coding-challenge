<template>
  <div class="button-container">
    <div
      class="button"
      :style="!previousPage ? 'visibility: hidden' : ''"
      @click="$emit('paginate', 'previous')"
    >
      Previous Page
    </div>
    <div>You are on Page: {{ currentPage }}</div>
    <div
      class="button"
      :style="!nextPage ? 'visibility: hidden' : ''"
      @click="$emit('paginate', 'next')"
    >
      Next Page
    </div>
  </div>
</template>

<script>
export default {
  name: "ButtonContainer",
  props: {
    previousPage: String,
    nextPage: String,
    currentPage: String,
  },
};
</script>