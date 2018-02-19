<template>
  <form>
    <h2>Filters</h2>
    <Radio id="sepia" name="filters" text="Sepia" checked="true" @valueUpdated="updateFilter"></Radio>
    <Radio id="grayscale" name="filters" text="Grayscale" checked="false" @valueUpdated="updateFilter"></Radio>
    <Radio id="no-filter" name="filters" text="#NoFilter" checked="false" @valueUpdated="updateFilter"></Radio>
    <!-- <input type="radio" id="sepia" name="filters" value="sepia" :checked="filter === 'sepia' ? 'checked' : null" @change="updateFilter">
    <label for="sepia">Sepia</label> -->
  </form>
</template>

<script>
  import Radio from './Radio';

  export default {
    name: 'Filters',
    components: { Radio },
    props: ['filter', 'change-filter'],
    methods: {
      updateFilter(e) {
        this.changeFilter(e.target.value);
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
