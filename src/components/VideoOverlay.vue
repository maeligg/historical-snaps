<template>
  <canvas class="canvas" width="500" height="375"></canvas>
</template>

<script>
  export default {
    name: 'VideoOverlay',
    props: ['accessories'],
    created() {
      this.renderAccessories(this.accessories);
    },
    methods: {
      renderAccessories(listOfAccessories) {
        console.log(listOfAccessories);
      }
    },
  };
</script>

<style scoped>
  .canvas {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
  }
</style>
