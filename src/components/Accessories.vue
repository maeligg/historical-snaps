<template>
  <form>
    <h2>Accessories</h2>
    <div class="accessories-wrapper">
      <Checkbox id="topHat" text="Top hat" @valueUpdated="updateAccessories"></Checkbox>
      <Checkbox id="bowlerHat" text="Bowler hat" @valueUpdated="updateAccessories"></Checkbox>
      <Checkbox id="cheBeret" text="El Che's beret" @valueUpdated="updateAccessories"></Checkbox>
      <Checkbox id="captainHat" text="Captain's hat" @valueUpdated="updateAccessories"></Checkbox>
      <Checkbox id="powderedWig" text="Powdered wig" @valueUpdated="updateAccessories"></Checkbox>
      <Checkbox id="handlebarMustache" text="Handlebar mustache" @valueUpdated="updateAccessories"></Checkbox>
      <Checkbox id="toothbrushMustache" text="Toothbrush mustache<br>(guaranteed 0% fascist)" @valueUpdated="updateAccessories"></Checkbox>
      <Checkbox id="daliMustache" text="Dali mustache" @valueUpdated="updateAccessories"></Checkbox>
      <Checkbox id="marxBeard" text="Marx beard" @valueUpdated="updateAccessories"></Checkbox>
      <Checkbox id="monocle" text="Monocle" @valueUpdated="updateAccessories"></Checkbox>
      <Checkbox id="smokingPipe" text="Smoking pipe" @valueUpdated="updateAccessories"></Checkbox>
      <Checkbox id="cthulhu" text="Fhtagn" @valueUpdated="updateAccessories"></Checkbox>
    </div>
  </form>
</template>

<script>
  import Checkbox from './Checkbox';

  export default {
    name: 'Accessories',
    components: { Checkbox },
    props: ['change-accessories'],
    methods: {
      updateAccessories(e) {
        this.changeAccessories(e.target.value, e.target.checked);
      },
    },
  };
</script>

<style lang="scss" scoped>
  h2 {
    font-family: 'Old English Five', sans-serif;
  }
</style>
