<template>
  <div class="webcam" :class="filter">
    <video class="video" width="500" height="400" preload autoplay loop muted></video>
    <canvas class="canvas" width="500" height="400" ref="canvas"></canvas>
    <img src="../assets/top-hat.png" alt="" ref="topHat" class="accessory">
    <img src="../assets/bowler-hat.png" alt="" ref="bowlerHat" class="accessory">
  </div>
</template>

<script>
  require('tracking');
  require('tracking/build/data/face');

  const tracker = new tracking.ObjectTracker('face');
  let canvas;
  let context;

  export default {
    name: 'Video',
    props: ['filter', 'accessories'],
    data() {
      return {
        topHat: {
          filterX: 0,
          filterY: -0.8,
          filterWidth: 1,
          filterHeight: 1,
        },
        bowlerHat: {
          filterX: 0,
          filterY: -0.8,
          filterWidth: 1,
          filterHeight: 1,
        },
      };
    },
    mounted() {
      this.initTracking();
      this.renderAccessories(this.accessories);
    },
    methods: {
      initTracking() {
        canvas = this.$refs.myCanvas;
        canvas = document.querySelector('.canvas');
        context = canvas.getContext('2d');
        tracker.setInitialScale(4);
        tracker.setStepSize(2);
        tracker.setEdgesDensity(0.1);
        tracking.track('.video', tracker, { camera: true });
      },
      renderAccessories(listOfAccessories) {
        tracker.on('track', (e) => {
          if (e.data.length) {
            context.clearRect(0, 0, canvas.width, canvas.height);

            listOfAccessories.forEach((accessory) => {
              const accessoryData = this.$data[accessory];
              const img = new Image();
              img.src = this.$refs[accessory].src;

              e.data.forEach((rect) => {
                context.drawImage(
                  img,
                  rect.x + (accessoryData.filterX * rect.width),
                  rect.y + (accessoryData.filterY * rect.height),
                  rect.width * accessoryData.filterWidth,
                  rect.height * accessoryData.filterHeight,
                );
              });
            });
          }
        });
      },
    },
  };
</script>

<style scoped>
  .webcam {
    position: relative;
    width: 500px;
    height: 400px;
  }

  .video {
    height: 100%;
    width: 100%;
    border: 2px solid #055e86;
  }

  .canvas {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
  }

  .grayscale {
    filter: grayscale(1);
  }

  .sepia {
    filter: sepia(1);
  }

  .accessory {
    display: none;
  }
</style>
