<template>
<div class="goods">
  <div class="menu-wrapper" ref="menuWrapper">
    <ul>
      <li v-for="(item,index) in goods" class="menu-item" :class="{'active': currentIndex === index}" @click="selectMenu(index)">
        <span class="text">
          <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
        </span>
      </li>
    </ul>
  </div>
  <div class="foods-wrapper" ref="foodsWrapper">
    <ul>
      <li v-for="item in goods" class="food-list food-list-hook">
        <h1 class="title">{{item.name}}</h1>
        <ul>
          <li v-for="food in item.foods" class="food-item" @click="selectedFood(food,$event)">
            <div class="food-icon">
              <img width="57" height="57" :src="food.icon">
            </div>
            <div class="content">
              <h2 class="name">{{food.name}}</h2>
              <p class="desc">{{food.description}}</p>
              <div class="extra">
                <span class="sellcount">月售{{food.sellCount}}份</span>
                <span>好评率{{food.rating}}%</span>
              </div>
              <div class="price">
                <span class="now">${{food.price}}</span>
                <span class="old" v-show="food.oldPrice">${{food.oldPrice}}</span>
              </div>
              <div class="cartcontrol-wrapper">
                <cartcontrol :food="food"></cartcontrol>
              </div>
            </div>
          </li>
        </ul>
      </li>
    </ui>
  </div>
  <shopcart ref="shopcart" v-bind:select-foods="selectFoods":delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
  <food :food="selectedFood" ref="food"></food>
</div>

</template>

<script type="text/ecmascript-6">
import BScroll from 'better-scroll';
import shopcart from 'components/shopcart/shopcart.vue';
import cartcontrol from 'components/cartcontrol/cartcontrol.vue';
import food from 'components/food/food.vue';
const ERR_OK = 0;
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      goods: [],
      listHeight: [],
      scrollY: 0,
      selectedFood: {}
    };
  },
  computed: {
    currentIndex () {
      for (let i = 0; i < this.listHeight.length; i++) {
        let height1 = this.listHeight[i];
        let height2 = this.listHeight[i + 1];
        if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
          return i;
        }
      }
      return 0;
    },
    selectFoods () {
      let foods = [];
      this.goods.forEach((good) => {
        good.foods.forEach((food) => {
          if (food.count) {
            foods.push(food);
          }
        });
      });
      return foods;
    }
  },
  created () {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    this.$http.get('/api/goods').then((response) => {
      response = response.body;
      if (response.errno === ERR_OK) {
        this.goods = response.data;
        this.$nextTick(() => {
          this._initScroll();
          this._calculateHeight();
        });
      }
    });
  },
  methods: {
    selectMenu (index) {
      let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
      let ref = foodList[index];
      this.foodsScroll.scrollToElement(ref, 300);
    },
    selectedFood (food, event) {
      if (!event._constructed) {
        return;
      }
      this.selectedFood = food;
      this.$refs.food.show();
    },
    _drop (target) {
      this.$refs.shopcart.drop(target);
    },
    _initScroll () {
      this.menuScroll = new BScroll(this.$refs.menuWrapper, {click: true});
      this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {click: true, probeType: 3});
      this.foodsScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y));
      });
    },
    _calculateHeight () {
      let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
      let height = 0;
      this.listHeight.push(height);
      for (let i = 0; i < foodList.length; i++) {
        let item = foodList[i];
        height += item.clientHeight;
        this.listHeight.push(height);
      }
    }

  },
  components: {
    cartcontrol,
    shopcart,
    food
  },
  event: {
    'cart.add' (target) {
      this._drop(target);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.goods{
  display: flex;
  position: absolute;
  top: 174px;
  bottom: 46px;
  width: 100%;
  overflow: hidden;
}
.menu-wrapper{
  flex: 0 0 80px;
  width: 80px;
  background: #f3f5f7;
}
.menu-item{
  display: table;
  height: 54px;
  width: 56px;
  line-height: 14px;
  padding: 0 12px;

}
li.active{
  position: relative;
  z-index: 10;
  font-weight: 700;
  background: #fff;
  margin-top: -1px;
  color: #0089dc;
}
.menu-item .text{
  display: table-cell;
  width: 56px;
  vertical-align: middle;
  font-size: 12px;
  border-bottom: 1px solid rgba(7,17,27,0.1);
}
.foods-wrapper{
    flex: 1;
}
.foods-wrapper .title{
  padding-left: 14px;
  height: 26px;
  line-height: 26px;
  border-left:2px solid #d9dde1;
  font-size: 12px;
  color: rgb(147,153,159);
  background: #f3f5f7;
}
.foods-wrapper .food-item{
  display:flex;
  margin: 18px;
  padding-bottom:18px;
  border-bottom: 1px solid rgba(7,17,27,0.1);
}
.foods-wrapper .food-item .food-icon{
  flex: 0 0 57px;
  margin-right: 10px;
}
.foods-wrapper .food-item .content{
  flex: 1;
  position: relative;
}
.foods-wrapper .food-item .content .name{
  margin: 2px 0 8px 0;
  height: 14px;
  line-height: 14px;
  font-size: 14px;
  color: rgb(7,17,27);
}
.foods-wrapper .food-item .content .desc{
  margin-bottom: 8px;
  line-height: 12px;
  font-size: 10px;
  color: rgb(147,153,159);
}
.foods-wrapper .food-item .content .extra{
  line-height: 10px;
  font-size: 10px;
  margin-bottom: 8px;
}
.foods-wrapper .food-item .content .extra .sellcount{
  margin-right: 12px;
}
.foods-wrapper .food-item .content .price{
  font-weight: 700;
  line-height: 24px;
}
.foods-wrapper .food-item .content .cartcontrol-wrapper{
  position: absolute;
  right: 0;
  bottom: 1px;
}
.foods-wrapper .food-item .content .price .now{
  margin-right: 8px;
  font-size: 14px;
  color: rgb(240,20,20);
  font-weight: 700;
}
.foods-wrapper .food-item .content .price .old{
  text-decoration: line-through;
  font-size: 10px;
  color: rgb(147,153,159);
}

</style>
