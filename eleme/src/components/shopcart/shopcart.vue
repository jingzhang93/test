<template>
  <div class="shopcart">
    <div class="content" @click="toggleList">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight':totalCount > 0 }">
            <icon  name="shopping_cart" scale="3" :class="{'highlight':totalCount > 0 }">
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'price-highlight':totalPrice > 0 }">${{totalPrice}}</div>
        <div class="desc">另需配送费{{deliveryPrice}}元</div>
      </div>
      <div class="content-right">
        <div class="pay" :class="payClass">{{payDesc}}</div>
      </div>
    </div>
    <div class="shopcart-list" v-show="listShow">
      <div class="list-header">
        <h1 class="title">购物车</h1>
        <span class="empty">清空</span>
      </div>
      <div class="list-content">
        <ul>
          <li class="food" v-for="food in selectFoods">
            <span class="name">{{food.name}}</span>
            <div class="price">
              <span>${{food.price*food.count}}</span>
            </div>
            <div class="cartcontrol-wrapper">
              <cartcontrol :food="food"></cartcontrol>
            </div>
          </li>
        </ul>
      </div>
    </div>
    <div class="ball-container">
      <div transition="drop" class="ball" v-for="ball in balls" v-show="ball.show">
        <div class="inner"></div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import cartcontrol from 'components/cartcontrol/cartcontrol.vue';
  export default {
    props: {
      selectFoods: {
        type: Array,
        default () {
          return [
            {
              price: 30,
              count: 1
            }
          ];
        }
      },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      }
    },
    data () {
      return {
        balls: [
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          }
        ],
        dropBall: []
      };
    },
    computed: {
      totalPrice () {
        let total = 0;
        this.selectFoods.forEach((food) => {
          total += food.price * food.count;
        });
        return total;
      },
      totalCount () {
        let count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count;
        });
        return count;
      },
      payDesc () {
        if (this.totalPrice === 0) {
          return '还差' + this.minPrice + '元起送';
        } else if (this.totalPrice < this.minPrice) {
          return '还差' + (this.minPrice - this.totalPrice) + '元配送';
        } else {
          return '去结算';
        }
      },
      payClass () {
        if (this.totalPrice < this.minPrice) {
          return 'not-enough';
        } else {
          return 'enough';
        }
      },
      listShow () {
        if (!this.totalCount) {
          this.fold = true;
          return false;
        }
        let show = !this.fold;
        return show;
      }
    },
    components: {
      cartcontrol
    },
    methods: {
      toggleList () {
        if (!this.totalCount) {
          return;
        }
        this.fold = !this.fold;
      },
      drop (el) {
        for (let i = 0; i < this.balls.length; i++) {
          let ball = this.ball[i];
          if (!ball.show) {
            ball.show = true;
            ball.el = el;
            this.dropBall.push(ball);
            return;
          }
        }
      }
    }
  };
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .shopcart{
    position: fixed;
    left: 0;
    bottom: 0;
    z-index: 50;
    width:100%;
    height: 48px;
    background: #141d27;
  }
  .shopcart .content{
    display: flex;
    background: #141d27;
  }
  .shopcart .content .content-left{
    flex: 1;
    color: rgba(255,255,255,0.4);
  }
  .shopcart .content .content-left .logo-wrapper{
    display: inline-block;
    position: relative;
    top: -10px;
    margin: 0 12px;
    padding: 6px;
    width: 56px;
    height: 56px;
    box-sizing: border-box;
    vertical-align: top;
    border-radius: 50%;
    background: #141d27;
  }
  .shopcart .content .content-left .logo-wrapper .logo{
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background: #2b343c;
    text-align: center;
    line-height: 3.6;
    color: #80858a
  }
  .highlight{
    background: rgb(0,160,220);
    border-radius: 50%;
    text-align: center;
    width: 100%;
    height: 100%;
    color: #fff;
    line-height: 3.6;
  }
  .shopcart .content .content-left .logo-wrapper .num{
    position: absolute;
    top: 0;
    right: 0;
    width: 24px;
    height: 16px;
    line-height: 16px;
    font-size: 9px;
    border-radius: 16px;
    text-align: center;
    font-weight: 700;
    color: #fff;
    background: rgb(240,20,20);
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);
  }
  .shopcart .content .content-left .price{
    display: inline-block;
    line-height:24px;
    vertical-align: top;
    margin-top: 12px;
    box-sizing: border-box;
    padding-right: 12px;
    border-right: 1px solid rgba(255,255,255,0.1);
    font-size: 16px;
    font-weight: 700;

  }
  .price-highlight{
    color: #fff;
  }
  .shopcart .content .content-left .desc{
    display: inline-block;
    line-height:24px;
    vertical-align: top;
    margin-left: 12px;
    margin: 12px 0 0 12px;
    font-size: 10px;
    font-weight: 700;
    color: rgba(255,255,255,0.4);
  }
  .shopcart .content .content-right{
    flex: 0 0 105px;
    width: 105px;
  }
  .content-right .pay{
    height: 48px;
    line-height: 48px;
    text-align: center;
    font-size: 12px;
    font-weight: 700;
  }
  .not-enough{
    background: #2b333b;
    color: rgba(255,255,255,0.4);
  }
  .enough{
    background: #00b43c;
    color: #fff;
  }
  .shopcart-list{
    position: absolute;
    top:0;
    left:0;
    z-index:-1;
    width:100%;
  }
  .list-header{
    height: 40px;
    line-height: 40px;
    padding:0 18px;
    background: #f3f5f7;
    border-bottom: 1px solid rgba(7,17,27,0.1);
  }
  .list-header .title{
    float: left;
    font-size: 14px;
    color:rgb(7,17,27);
  }
  .list-header .empty{
    float: right;
    font-size: 12px;
    color:rgb(0,160,220);
  }
  .list-content{
    padding: 0 18px;
    max-height: 217px;
    background: #fff;
    overflow: hidden;
  }
  .ball-container .ball{
    position: fixed;
    left: 32px;
    bottom: 22px;
    z-index: 200;
  }
</style>
