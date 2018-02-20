<template>
  <div class="input-wrapper">
    <input type="checkbox" :id="id" :value="id" @change="updateValue">
    <label :for="id" v-html="text"></label>
  </div>
</template>

<script>
  export default {
    name: 'Chexbox',
    props: ['id', 'name', 'text'],
    methods: {
      updateValue(e) {
        this.$emit('valueUpdated', e);
      },
    },
  };
</script>

<style lang="scss" scoped>
  .input-wrapper {
    display: flex;
    margin-bottom: 10px;
  }

  input[type='checkbox'] {
    position: relative;
    display: inline-block;
    appearance: none;
    width: 40px;
    height: 20px;
    margin-bottom: 10px;
    background-color: #fff;
    border-radius: 30px;
    cursor: pointer;

    &:after{
      content: '';
      display: inline-block;
      position: absolute;
      width: 16px;
      height: 16px;
      background-color: hsl(0, 0%, 50%);
      top: 2px;
      left: 2px;
      border-radius: 50%;
      transition: all .3s ease-in-out;
    }

    &:checked {
      box-shadow: inset 0 0 0 15px hsla(0,0%,0%,.75);;
      &:after{
        left:21px;
        background-color:#fff;
      }
    }

    + label {
      margin-left: 10px;
      cursor: pointer;
    }
  }
</style>

