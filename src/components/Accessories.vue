<template>
  <form>
    <h2>Accessories</h2>
    <input type="checkbox" id="top-hat" value="topHat" @change="updateAccessories">
    <label for="top-hat">Top hat</label>
    <input type="checkbox" id="bowler-hat" value="bowlerHat" @change="updateAccessories">
    <label for="bowler-hat">Bowler hat</label>
    <input type="checkbox" id="handlebar-mustache" value="handlebarMustache" @change="updateAccessories">
    <label for="handlebar-mustache">Handlebar mustache</label>
    <input type="checkbox" id="toothbrush-mustache" value="toothbrushMustache" @change="updateAccessories">
    <label for="toothbrush-mustache">Toothbrush mustache (guaranteed 0% fascist)</label>
    <input type="checkbox" id="dali-mustache" value="daliMustache" @change="updateAccessories">
    <label for="dali-mustache">Dali mustache</label>
    <input type="checkbox" id="monocle" value="monocle" @change="updateAccessories">
    <label for="monocle">Monocle</label>
    <input type="checkbox" id="smoking-pipe" value="smokingPipe" @change="updateAccessories">
    <label for="smoking-pipe">Smoking pipe</label>
    <input type="checkbox" id="che-beret" value="cheBeret" @change="updateAccessories">
    <label for="che-beret">El Che's beret</label>
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
