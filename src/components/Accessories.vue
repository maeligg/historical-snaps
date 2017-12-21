<template>
  <form>
    <h2>Accessories</h2>
    <div class="input-wrapper">
      <input type="checkbox" id="top-hat" value="topHat" @change="updateAccessories">
      <label for="top-hat">Top hat</label>
    </div>
    <div class="input-wrapper">
      <input type="checkbox" id="bowler-hat" value="bowlerHat" @change="updateAccessories">
      <label for="bowler-hat">Bowler hat</label>
    </div>
    <div class="input-wrapper">
      <input type="checkbox" id="handlebar-mustache" value="handlebarMustache" @change="updateAccessories">
      <label for="handlebar-mustache">Handlebar mustache</label>
    </div>
    <div class="input-wrapper">
      <input type="checkbox" id="toothbrush-mustache" value="toothbrushMustache" @change="updateAccessories">
      <label for="toothbrush-mustache">Toothbrush mustache<br>(guaranteed 0% fascist)</label>
    </div>
    <div class="input-wrapper">
      <input type="checkbox" id="dali-mustache" value="daliMustache" @change="updateAccessories">
      <label for="dali-mustache">Dali mustache</label>
    </div>
    <div class="input-wrapper">
      <input type="checkbox" id="monocle" value="monocle" @change="updateAccessories">
      <label for="monocle">Monocle</label>
    </div>
    <div class="input-wrapper">
      <input type="checkbox" id="smoking-pipe" value="smokingPipe" @change="updateAccessories">
      <label for="smoking-pipe">Smoking pipe</label>
    </div>
    <div class="input-wrapper">
      <input type="checkbox" id="che-beret" value="cheBeret" @change="updateAccessories">
      <label for="che-beret">El Che's beret</label>
    </div>
  </form>
</template>

<script>
  export default {
    name: 'Accessories',
    props: ['change-accessories'],
    methods: {
      updateAccessories(e) {
        this.changeAccessories(e.target.value, e.target.checked);
      },
    },
  };
</script>

<style lang="scss" scoped>
  h2 {
    font-family: 'Old English Five', sans-serif;
  }

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
