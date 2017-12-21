<template>
  <section class="webcam" :class="filter">
    <div class="foreground" :class="foreground"></div>
    <video class="video" width="500" height="400" preload autoplay loop muted></video>
    <canvas class="canvas" width="500" height="400" ref="canvas"></canvas>
    <img src="../assets/top-hat.png" alt="" ref="topHat" class="accessory">
    <img src="../assets/bowler-hat.png" alt="" ref="bowlerHat" class="accessory">
    <img src="../assets/handlebar-mustache.png" alt="" ref="handlebarMustache" class="accessory">
    <img src="../assets/toothbrush-mustache.png" alt="" ref="toothbrushMustache" class="accessory">
    <img src="../assets/dali-mustache.png" alt="" ref="daliMustache" class="accessory">
    <img src="../assets/monocle.png" alt="" ref="monocle" class="accessory">
    <img src="../assets/smoking-pipe.png" alt="" ref="smokingPipe" class="accessory">
    <img src="../assets/che-beret.png" alt="" ref="cheBeret" class="accessory">
  </section>
</template>

<script>
  require('tracking');
  require('tracking/build/data/face');

  const tracker = new tracking.ObjectTracker('face');
  let canvas;
  let context;

  export default {
    name: 'Video',
    props: ['filter', 'accessories', 'foreground'],

    data() {
      return {
        topHat: {
          filterX: 0,
          filterY: -0.7,
          filterWidth: 1,
          filterHeight: 1,
        },
        bowlerHat: {
          filterX: 0,
          filterY: -0.65,
          filterWidth: 1,
          filterHeight: 1,
        },
        handlebarMustache: {
          filterX: 0.27,
          filterY: 0.48,
          filterWidth: 0.5,
          filterHeight: 0.5,
        },
        toothbrushMustache: {
          filterX: 0.42,
          filterY: 0.65,
          filterWidth: 0.2,
          filterHeight: 0.2,
        },
        daliMustache: {
          filterX: 0.23,
          filterY: 0.4,
          filterWidth: 0.6,
          filterHeight: 0.6,
        },
        monocle: {
          filterX: -0.15,
          filterY: 0.3,
          filterWidth: 0.9,
          filterHeight: 0.9,
        },
        smokingPipe: {
          filterX: 0.3,
          filterY: 0.7,
          filterWidth: 0.6,
          filterHeight: 0.6,
        },
        cheBeret: {
          filterX: 0,
          filterY: -0.6,
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

<style lang="scss" scoped>
  .webcam {
    position: relative;
    width: 500px;
    height: 400px;
    margin-right: 40px;
    border: 2px solid #000;
  }

  .foreground, .canvas, .video {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
  }

  .foreground {
    z-index: 10;

    &.comrades {
      background-image: url('../assets/comrades.png');
    }
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
