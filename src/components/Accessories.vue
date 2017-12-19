<template>
  <form>
    <h2>Accessories</h2>
    <input type="checkbox" id="top-hat" value="topHat" @change="updateAccessories">
    <label for="top-hat">Top hat</label>
    <input type="checkbox" id="bowler-hat" value="bowlerHat" @change="updateAccessories">
    <label for="bowler-hat">Bowler hat</label>
  </form>
</template>

<script>
  export default {
    name: 'Accessories',
    props: ['change-accessories'],
    methods: {
      updateAccessories(e) {
        this.changeAccessories(e.target.value, e.target.checked);
      },
    },
  };
</script>

<style scoped>
  h2, label {
    color: #fff;
  }
</style>
