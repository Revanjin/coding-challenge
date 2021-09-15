<template>
  <header>
    <div class="headline__container">
      <h1 class="headline headline--stroke headline--shadow">{{ title }}</h1>
    </div>
  </header>
</template>

<script>
export default {
  name: "Header",
  props: {
    title: String,
  },
};
</script>