<template>
  <div v-show="showFlag" class="food">
    <div class="food-content">
      <div class="image-header">
        <img :src="food.image">
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      food: {
        type: Object
      }
    },
    data () {
      return {
        showFlag: false
      };
    },
    methods: {
      show () {
        this.showFlag = true;
      }
    }
  };
</script>

<style>
  .food {
    position: fixed;
    left: 0;
    top: 0;
    bottom: 48px;
    z-index: 30;
    width: 100%;
    background: #fff;
  }
  .image-header {
    position: relative;
    width: 100%;
    height: 0;
    padding-top: 100%;
  }
  .image-header img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }
</style>
