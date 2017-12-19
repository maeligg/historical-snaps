<template>
  <div id="app">
    <div class="container">
      <Video :filter="selectedFilter" :accessories="accessories"></Video>
      <Filters :filter="selectedFilter" :change-filter="changeFilter"></Filters>
      <Accessories :change-accessories="changeAccessories"></Accessories>
    </div>
  </div>
</template>

<script>
  import 'normalize.css';
  import Video from './components/Video';
  import Filters from './components/Filters';
  import Accessories from './components/Accessories';

  export default {
    name: 'app',
    components: {
      Video,
      Filters,
      Accessories,
    },
    data() {
      return {
        selectedFilter: 'sepia',
        accessories: [],
      };
    },
    methods: {
      changeFilter(filter) {
        this.selectedFilter = filter;
      },
      changeAccessories(accessory, checked) {
        if (checked) {
          this.accessories.push(accessory);
        } else {
          const index = this.accessories.indexOf(accessory);
          if (index > -1) {
            this.accessories.splice(index, 1);
          }
        }
      },
    },
  };
</script>

<style scoped>
  #app {
    min-height: 100vh;
    background-color: #055e86;
  }

  .container {
    max-width: 900px;
    margin: 0 auto;
  }
</style>
