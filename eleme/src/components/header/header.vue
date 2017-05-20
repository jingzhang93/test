<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar"><img width="64" height="64"  :src="seller.avatar"></div>
      <div class="content">
        <div class="title">
          <span class="brand"><img src="../../assets/brand@2x.png"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon"><img src="../../assets/decrease_1@2x.png"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}</span>
        <icon name="keyboard_arrow_right" scale="2">
      </div>
      </div>

    </div>
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"><img src="../../assets/bulletin@2x.png"></span><span class="bulletin-text">{{seller.bulletin}}</span>
    </div>

    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>

    <div class="detail" v-show="detailShow" transition="fade">
      <div class="detail-wrapper clearfix">
        <div class="detail-main">
          <h1 class="name">{{seller.name}}</h1>
          <div class="title">
            <div class="line"></div>
            <div class="text">优惠信息</div>
            <div class="line"></div>
          </div>
          <ul v-if="seller.supports" class="supports">
            <span class="icon"><img src="../../assets/decrease_2@2x.png"></span>
            <span class="text">{{seller.supports[0].description}}</span>
          </ul>
          <div class="title">
            <div class="line"></div>
            <div class="text">商家公告</div>
            <div class="line"></div>
          </div>
          <div class="bulletin">
            <p class="content">{{seller.bulletin}}</p>
          </div>
        </div>
      </div>
      <div class="detail-close" @click="hiddenDetail"><icon name="close" scale="4"></icon></div>
    </div>

  </div>
</template>

<script type="text/ecmascript-6">

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        detailShow: false
      };
    },
    methods: {
      showDetail () {
        this.detailShow = true;
      },
      hiddenDetail () {
        this.detailShow = false;
      }
    }
  };
</script src="../../common/stylus/icon.styl">

<style>

  .header{
    position: relative;
    color: rgb(255,255,255);
    background: rgba(7,17,27,0.5);
    overflow:hidden;


  }
  .content-wrapper{
    position: relative;
    padding: 24px 12px 18px 24px;
    font-size: 0;
  }
  .avatar{
    display: inline-block;
    vertical-align: top;
  }
  .avatar img{
    border-radius: 2px;
  }
  .content{
    display: inline-block;
    margin-left: 16px;
    font-size: 14px;
  }
  .title{
    margin:2px 0 8px 0;
  }
  .name{
    font-size:16px;
    font-weight:bold;
    line-height:18px;
    margin-left: 6px;
  }
  .brand img{
    vertical-align: top;
    display:inline-block;
    width: 30px;
    height: 18px;
    background-repeat: no-repeat;
  }
  .description{
    font-size: 12px;
    line-height: 12px;
    font-weight: 200;
    margin-bottom: 10px;
  }
  .icon img{
    vertical-align: top;
    display: inline-block;
    width: 12px;
    height: 12px;
    margin-right: 4px;
    background-size: 12px 12px;
    background-repeat: no-repeat;
  }
  .text{
    font-size: 10px;
    line-height: 12px;
    font-weight: 200;
  }
  .support-count{
    position: absolute;
    right: 12px;
    bottom: 14px;
    padding : 0 8px;
    height:24px;
    line-height: 24px;
    border-radius: 14px;
    background: rgba(0,0,0,0.2);
    text-align: center;
  }
  .bulletin-wrapper{
    position:relative;
    height: 28px;
    line-height: 28px;
    padding: 0 22px 0 12px;
    white-space:nowrap;
    overflow: hidden;
    text-overflow:ellipsis;
    background-color: rgba(7,17,27,0.2);

  }
  .bulletin-title img{
    display: inline-block;
    vertical-align: top;
    margin-top:8px;
    width:22px;
    height: 12px;
    background-size: 22px 12px;
    background-repeat: no-repeat;
  }
  .bulletin-text{

    margin:0 4px;
    font-size: 10px;
    font-weight:200;
  }
  .background{
    position: absolute;
    top:0;
    left:0;
    width:100%;
    height:100%;
    z-index:-1;
    filter: blur(10px);
  }
  .detail{
    position:fixed;
    z-index: 100;
    width:100%;
    height:100%;
    overflow: auto;
    background: rgba(7,17,27,0.8);
    top:0;
    left:0;
  }
  .clearfix{
    display: inline-block;
  }
  .clearfix &:after{
    display: block;
    content: ".";
    height:0;
    line-height:0;
    clear:both;
    visibility:hidden;
  }
  .detail-wrapper{
    min-height: 100%;
    width: 100%;
  }
  .detail-main{
    margin-top:64px;
    padding-bottom: 64px;

  }
  .detail-main .name{
    line-height: 16px;
    text-align: center;
    font-size: 16px;
    font-weight: 700;
  }
  .detail-main .title{
    display :flex;
    width: 80%;
    margin: 28px auto 24px auto;
  }
  .detail-main .title .line{
    flex: 1;
    position: relative;
    top: -6px;
    border-bottom: 1px solid rgba(255,255,255,0.2);
  }
  .detail-main .title .text{
    padding: 0 12px;
    font-size: 14px;
    font-weight: 700;
  }
  .detail-main .bulletin{
    width: 80%;
    margin: 0 auto;

  }
  .detail-main .bulletin .content{
    padding: 0 12px;
    line-height: 24px;
    font-size: 12px;
  }
  .detail-close{
    position:relative;
    width: 32px;
    height: 32px;
    margin: -64px auto 0 auto;
    clear: both;
    font-size: 32px;
  }



</style>
