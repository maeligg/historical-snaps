<template>
  <div>
    <section class="webcam" :class="filter">
    <div class="foreground" :class="foreground" ref="foreground"></div>
    <video class="video" width="500" height="400" preload autoplay loop muted ref="video"></video>
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
  <button class="snapshot-button" @mouseover="randomiseText" @click="takePicture">
      Take the shot
      <div class="photographer-wrapper">
        <img src="../assets/photographer.svg" alt="photographer" class="photographer">
        <div class="photographer-bubble">{{ photographerText }}</div>
      </div>
    </button>

    <div class="pictures" ref="pictures"></div>
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
        photographerText: '',
        textArray: ['Beautiful !', 'Gimme a smile !', 'Stand still now.', 'Don\'t move.'],
      };
    },

    mounted() {
      this.initTracking();
      this.renderAccessories(this.accessories);
    },

    methods: {
      initTracking() {
        canvas = this.$refs.canvas;
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
      randomiseText: function randomText() {
        this.photographerText = this.textArray[Math.floor(Math.random() * this.textArray.length)];
      },
      takePicture: function takePic() {
        const picCanvas = document.createElement('canvas');
        picCanvas.width = 500;
        picCanvas.height = 400;
        const ctx = picCanvas.getContext('2d');

        if (this.filter !== 'no-filter') {
          ctx.filter = `${this.filter}(100)`;
        }
        ctx.drawImage(this.$refs.video, 0, 0, canvas.width, canvas.height);
        ctx.drawImage(this.$refs.canvas, 0, 0, canvas.width, canvas.height);

        const dataURI = canvas.toDataURL('image/jpeg');

        const newPic = new Image();
        newPic.src = dataURI;
        newPic.style.cssText = 'width: 100px; margin: .5rem;';
        this.$refs.pictures.appendChild(newPic);
      },
    },
  };
</script>

<style lang="scss" scoped>
  .webcam {
    position: relative;
    width: 100%;
    height: 400px;
    margin: 0 auto;
    overflow: hidden;
  }

  .foreground, .canvas, .video {
    position: absolute;
    top: 0;
    left: 0;
  }

  .foreground {
    width: 500px;
    height: 100%;
    z-index: 10;
    background-size: cover;
    background-repeat: no-repeat;

    &.frame {
      background-image: url('../assets/frame.png');
    }

    &.comrades {
      background-image: url('../assets/comrades.png');
    }

    &.boat-wheel {
      background-image: url('../assets/boat-wheel.png');
      background-position: center bottom;
      background-size: 70%;
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

  .snapshot-button {
    position: relative;
    z-index: 10;
    display: block;
    margin: 1rem auto;
    padding: .5rem 1rem;
    border: 1px solid #000;;
    border-radius: 4px;
    background-color: #af845a;
    cursor: pointer;

    &::before {
      content: "";
      position: absolute;
      top: -6px;
      right: 10px;
      width: 10px;
      height: 6px;
      background-color: #000;
      transition: all .2s ease-in-out;
    }

    &:hover {
      &::before {
        top: -2px;
        height: 2px;
      }

      @media (min-width: 800px) {
        .photographer-wrapper {
          left: 0;
        }

        .photographer-bubble {
          opacity: 1;
        }
      }
    }
  }

  .photographer-wrapper {
    position: fixed;
    bottom: 0;
    left: -200px;
    width: 200px;
    transition: all .2s ease-in-out;
  }

  .photographer-bubble {
    opacity: 0;
    position: absolute;
    top: -40px;
    right: 0;
    padding: 2rem;
    border: 1px solid #000;
    border-radius: 50%;
    background-color: #fff;
    transition: all .2s ease-in-out;
    transition-delay: .4s;

    &::before {
      content: "";
      position: absolute;
      bottom: -10px;
      right: 50%;
      height: 1px;
      transform: rotate(-45deg);
      width: 30px;
      background-color: #000;
    }
  }

  .picture {
    width: 100px;
  }
</style>
