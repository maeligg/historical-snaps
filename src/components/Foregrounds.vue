<template>
  <form>
    <h2>Foregrounds</h2>
    <Radio id="none" text="None" name="foreground" @valueUpdated="updateForeground"></Radio>
    <Radio id="frame" text="Frame" name="foreground" @valueUpdated="updateForeground"></Radio>
    <Radio id="boat-wheel" text="Boat wheel" name="foreground" @valueUpdated="updateForeground"></Radio>
    <Radio id="comrades" text="Chillin' with my comrades" name="foreground" @valueUpdated="updateForeground"></Radio>
  </form>
</template>

<script>
  import Radio from './Radio';

  export default {
    name: 'Foregrounds',
    components: { Radio },
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
