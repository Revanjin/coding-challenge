<template>
  <div v-if="!isLoading" class="planet__container">
    <div class="tooltip">
      <div class="tooltip__container">Click the planet</div>
    </div>
    <div
      class="planet"
      @click="$emit('toggle-information')"
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
export default {
  name: "Planet",
  props: {
    isLoading: Boolean,
  },
  emit: ["toggle-information"],
};
</script>