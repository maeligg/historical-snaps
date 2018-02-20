<template>
  <form>
    <h2>Foregrounds</h2>
    <RadioButton id="none" text="None" name="foreground" @valueUpdated="updateForeground"></RadioButton>
    <RadioButton id="frame" text="Frame" name="foreground" @valueUpdated="updateForeground"></RadioButton>
    <RadioButton id="boat-wheel" text="Boat wheel" name="foreground" @valueUpdated="updateForeground"></RadioButton>
    <RadioButton id="comrades" text="Chillin' with my comrades" name="foreground" @valueUpdated="updateForeground"></RadioButton>
  </form>
</template>

<script>
  import RadioButton from './RadioButton';

  export default {
    name: 'Foregrounds',
    components: { RadioButton },
    props: ['change-foreground'],
    methods: {
      updateForeground(e) {
        this.changeForeground(e.target.value);
      },
    },
  };
</script>

<style lang="scss" scoped>
  h2 {
    font-family: 'Old English Five', sans-serif;
  }
</style>
