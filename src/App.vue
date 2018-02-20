<template>
  <div id="app">
    <div class="container">
      <h1>Historical <img src="./assets/camera.png" alt="camera" class="camera"> Snaps</h1>
      <div class="main-wrapper">
        <Video :filter="selectedFilter" :accessories="accessories" :foreground="selectedForeground"></Video>
        <section class="sidebar">
          <Filters :filter="selectedFilter" :change-filter="changeFilter"></Filters>
          <Accessories :change-accessories="changeAccessories"></Accessories>
          <Foregrounds :change-foreground="changeForeground"></Foregrounds>
        </section>
      </div>
    </div>
  </div>
</template>

<script>
  import 'normalize.css';
  import Video from './components/Video';
  import Filters from './components/Filters';
  import Accessories from './components/Accessories';
  import Foregrounds from './components/Foregrounds';

  export default {
    name: 'app',
    components: {
      Video,
      Filters,
      Accessories,
      Foregrounds,
    },
    data() {
      return {
        selectedFilter: 'noFilter',
        accessories: [],
        selectedForeground: '',
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
      changeForeground(foreground) {
        this.selectedForeground = foreground;
      },
    },
  };
</script>

<style lang="scss" scoped>
  @import url('https://fonts.googleapis.com/css?family=Roboto');

  @font-face {
    font-family: 'Old English Five';
    src: url('./assets/fonts/OldEnglishFive.woff2') format('woff2'),
         url('./assets/fonts/OldEnglishFive.woff') format('woff');
    font-weight: normal;
    font-style: normal;
  }

  * {
    font-family: 'Roboto', sans-serif;
  }

  #app {
    padding: 2rem 0;
    box-shadow: inset 0px 0px 200px 10px hsla(0,0%,0%,.75);
    background-color: #E2B88E;
  }

  .container {
    max-width: 900px;
    margin: 0 auto;
  }

  h1 {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    margin-top: 0;
    font-size: 3rem;
    font-family: 'Old English Five', sans-serif;
  }

  .camera {
    width: 150px;
    margin: 2rem;
  }

  .main-wrapper {
    display: grid;
    grid-template-columns: 2fr 1fr;
    grid-gap: 20px;

    @media (max-width: 800px) {
      grid-template-columns: 1fr;
      grid-gap: 0;
    }
  }

  .sidebar {
    padding: 0 2rem;
    grid-column: 2 / -1;
    grid-row: 1 / 4;

    @media (max-width: 800px) {
      grid-column: 1 / -1;
      grid-row: 3
    }
  }

  .snapshot {
    grid-column: 1;
    grid-row: 2;

    @media (max-width: 800px) {
      grid-row: 2;
    }
  }
</style>
