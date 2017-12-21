<template>
  <form>
    <h2>Foregrounds</h2>
    <input type="radio" id="comrades" name="filters" value="comrades" @change="updateForeground">
    <label for="comrades">Chillin' with my comrades</label>
  </form>
</template>

<script>
  export default {
    name: 'Foregrounds',
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

  input[type='radio'] {
    display: none;
    cursor: pointer;

    + label {
      cursor: pointer;
      display: inline-block;
      position: relative;
      padding-left: 25px;
      margin-right: 10px;

      &:before, &:after {
        content: '';
        display: inline-block;
        width: 18px;
        height: 18px;
        left: 0;
        bottom: 0;
        text-align: center;
        position: absolute;
      }

      &:before {
        border-radius: 50%;
        background-color: #fff;
      }

      &:after {
        color: #fff;
      }
    }

    + label:hover:after,
    &:checked + label:after {
      content:'\2022';
      position:absolute;
      top:1px;
      font-size:40px;
      line-height:20px;
    }

    &:checked + label:after,
    &:checked + label:hover:after{
      color:#000;
    }
  }
</style>
