<template>
  <div>
    <input type="radio" :id="id" :name="name" :value="id" @change="updateValue">
    <label :for="id">{{text}}</label>
  </div>
</template>

<script>
  export default {
    name: 'Radio',
    props: ['id', 'name', 'text'],
    methods: {
      updateValue(e) {
        this.$emit('valueUpdated', e);
      },
    },
  };
</script>

<style lang="scss" scoped>
  fieldset {
    padding: 0;
    border: none;
  }

  input[type='radio'] {
    display: none;
    cursor: pointer;

    + label {
      cursor: pointer;
      display: inline-block;
      position: relative;
      margin: 0 1rem 1rem 0;
      padding-left: 25px;

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
      //top: -1px;
      font-size:40px;
      line-height:20px;
    }

    &:checked + label:after,
    &:checked + label:hover:after{
      color:#000;
    }
  }
</style>

