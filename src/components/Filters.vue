<template>
  <form>
    <h2>Filters</h2>
    <input type="radio" id="sepia" name="filters" value="sepia" :checked="filter === 'sepia' ? 'checked' : null" @change="updateFilter">
    <label for="sepia">Sepia</label>
    <input type="radio" id="grayscale" name="filters" value="grayscale" :checked="filter === 'grayscale' ? 'checked' : null" @change="updateFilter">
    <label for="grayscale">Grayscale</label>
    <input type="radio" id="no-filter" name="filters" value="no-filter" :checked="filter === 'no-filter' ? 'checked' : null" @change="updateFilter">
    <label for="no-filter">#NoFilter</label>
  </form>
</template>

<script>
  export default {
    name: 'Filters',
    props: ['filter', 'change-filter'],
    methods: {
      updateFilter(e) {
        this.changeFilter(e.target.value);
      },
    },
  };
</script>

<style scoped>
  h2, label {
    color: #fff;
  }
</style>
