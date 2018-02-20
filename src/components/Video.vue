<template>
  <main>
    <section class="webcam" :class="filter">
      <video class="video" width="500" height="400" preload autoplay loop muted ref="video"></video>
      <canvas class="canvas" width="500" height="400" ref="canvas"></canvas>

      <img src="../assets/top-hat.png" alt="" ref="topHat" class="accessory">
      <img src="../assets/bowler-hat.png" alt="" ref="bowlerHat" class="accessory">
      <img src="../assets/che-beret.png" alt="" ref="cheBeret" class="accessory">
      <img src="../assets/captain-hat.png" alt="" ref="captainHat" class="accessory">
      <img src="../assets/powdered-wig.png" alt="" ref="powderedWig" class="accessory">
      <img src="../assets/handlebar-mustache.png" alt="" ref="handlebarMustache" class="accessory">
      <img src="../assets/toothbrush-mustache.png" alt="" ref="toothbrushMustache" class="accessory">
      <img src="../assets/marx-beard.png" alt="" ref="marxBeard" class="accessory">
      <img src="../assets/dali-mustache.png" alt="" ref="daliMustache" class="accessory">
      <img src="../assets/monocle.png" alt="" ref="monocle" class="accessory">
      <img src="../assets/smoking-pipe.png" alt="" ref="smokingPipe" class="accessory">

      <img src="../assets/frame.png" alt="" ref="frame" class="foreground">
      <img src="../assets/boat-wheel.png" alt="" ref="boat-wheel" class="foreground">
      <img src="../assets/comrades.png" alt="" ref="comrades" class="foreground">
    </section>

    <button class="snapshot-button" @mouseover="randomiseText" @click="takePicture">
      Take the shot
      <div class="photographer-wrapper">
        <img src="../assets/photographer.svg" alt="photographer" class="photographer">
        <div class="photographer-bubble">{{ photographerText }}</div>
      </div>
    </button>

    <div class="pictures" ref="pictures"></div>
  </main>
</template>

<script>
  import 'tracking';
  import 'tracking/build/data/face';
  import cameraSound from '../assets/camera-sound.mp3';

  const tracker = new tracking.ObjectTracker('face');
  let canvas;
  let context;

  export default {
    name: 'Video',
    props: ['filter', 'accessories', 'foreground'],
    data() {
      return {
        accessoriesData: {
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
          cheBeret: {
            filterX: -0.1,
            filterY: -0.6,
            filterWidth: 1.2,
            filterHeight: 1.1,
          },
          captainHat: {
            filterX: -0.1,
            filterY: -0.7,
            filterWidth: 1.2,
            filterHeight: 1,
          },
          powderedWig: {
            filterX: -0.05,
            filterY: -0.45,
            filterWidth: 1.2,
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
          marxBeard: {
            filterX: 0.03,
            filterY: 0.6,
            filterWidth: 1,
            filterHeight: 1,
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
        },
        photographerText: '',
        textArray: [
          'A beautiful pose !',
          'Give me a smile !',
          'Stand still now.',
          'Don\'t move.',
          'Hold still please.',
          'You look terrific.',
          'Look at the camera.',
        ],
      };
    },

    mounted() {
      this.initTracking();
      this.renderCanvas();
    },

    methods: {
      applyForground() {

      },

      initTracking() {
        tracker.setInitialScale(4);
        tracker.setStepSize(2);
        tracker.setEdgesDensity(0.1);
        tracking.track('.video', tracker, { camera: true });
      },

      renderCanvas() {
        canvas = this.$refs.canvas;
        context = canvas.getContext('2d');

        tracker.on('track', (e) => {
          if (e.data.length) {
            context.clearRect(0, 0, canvas.width, canvas.height);

            this.accessories.forEach((accessory) => {
              const accessoryData = this.$data.accessoriesData[accessory];
              const accessoryImg = new Image();
              accessoryImg.src = this.$refs[accessory].src;
              e.data.forEach((rect) => {
                context.drawImage(
                  accessoryImg,
                  rect.x + (accessoryData.filterX * rect.width),
                  rect.y + (accessoryData.filterY * rect.height),
                  rect.width * accessoryData.filterWidth,
                  rect.height * accessoryData.filterHeight,
                );
              });
            });

            if (this.foreground !== 'none') {
              const foregroundImg = new Image();
              foregroundImg.src = this.$refs[this.foreground].src;

              context.drawImage(
                foregroundImg,
                0,
                0,
                canvas.width,
                canvas.height,
              );
            }
          }
        });
      },
      randomiseText: function randomText() {
        this.photographerText = this.textArray[Math.floor(Math.random() * this.textArray.length)];
      },
      takePicture: function takePic() {
        const audio = new Audio(cameraSound);
        audio.play();

        const picCanvas = document.createElement('canvas');
        picCanvas.width = 500;
        picCanvas.height = 400;
        const ctx = picCanvas.getContext('2d');

        if (this.filter !== 'no-filter') {
          ctx.filter = `${this.filter}(100)`;
        }
        ctx.drawImage(this.$refs.video, 0, 0, picCanvas.width, picCanvas.height);
        ctx.drawImage(this.$refs.canvas, 0, 0, picCanvas.width, picCanvas.height);

        const dataURI = picCanvas.toDataURL('image/jpeg');

        const newPic = new Image();
        newPic.src = dataURI;
        const picWrapper = document.createElement('span');
        picWrapper.classList.add('picture');
        picWrapper.appendChild(newPic);
        picWrapper.addEventListener('click', function toggleOpen() {
          this.classList.toggle('open');
        });

        this.$refs.pictures.appendChild(picWrapper);

        this.randomiseText();
      },
    },
  };
</script>

<style lang="scss">
  main {
    width: 500px;
  }

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

  .grayscale {
    filter: grayscale(1);
  }

  .sepia {
    filter: sepia(1);
  }

  .accessory, .foreground {
    display: none;
  }

  .snapshot-button {
    position: relative;
    z-index: 10;
    display: block;
    margin: 2rem auto 1rem;
    padding: 3rem 1rem 1rem;
    border: none;
    border-radius: 4px;
    background-color: #000;
    color: #fff;
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

    &::after {
      content: "";
      position: absolute;
      top: 15px;
      left: 50%;
      transform: translateX(-50%);
      width: 20px;
      height: 20px;
      border-radius: 50%;
      border: 2px solid #fff;
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
    color: #000;
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
    cursor: pointer;

    img {
      width: 100px;
      margin: .5rem;
    }

    &.open {
      position: fixed;
      z-index: 100;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: hsla(0, 0, 0, .7);

      img {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        width: 500px;
        margin: 0;
      }
    }
  }
</style>
