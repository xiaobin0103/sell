<template>
  <div class="header">
   <div class="content-wrapper">
      <!--头像-->
      <div class="avatar">
      	<img width="64" height="64" :src="seller.avatar"/>
      </div>
      <!--内容-->
      <div class="content">
        <!--标题-->
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <!--描述-->
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <!--折扣信息-->
        <div v-if="seller.supports" class="supports">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>

        </div>
      </div>
      <!--点击弹出-->
     <div v-if="seller.supports" class="support-count" @click="showDetail">  
       <i class="icon-arrow_lift"></i>
       <span class="count">{{seller.supports.length}}个</span>

     </div>

    </div> 
    <!--公告-->
   <div class="bulletin-wrapper"  @click="showDetail">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-arrow_lift"></i>
   </div>
   <!--背景-->
   <div class="background">
     <img :src="seller.avatar" alt="" width="100%" height="100%">
   </div>

    <!--弹出层-->
  <div class="detail" v-show="detailShow" transition="fade">
    <div class="detail-warpper clearfix">
        <div class="detail-main">
           <h1 class="name">{{seller.name}}</h1>
           <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star> 
           </div>
           <!--优惠信息-->
           <div class="title">
             <div class="line"></div>
             <div class="text">优惠信息</div>
             <div class="line"></div>
           </div> 
           <!--列表 -->
           <ul v-if="seller.supports" class="supports">
             <li class="supports-item" v-for="item in seller.supports">
               <span class="icon" :class="classMap[item.type]"></span>
               <span class="text">{{item.description}}</span>
             </li>
           </ul>
           <!--商家公告-->
           <div class="title">
             <div class="line"></div>
             <div class="text">商家公告</div>
             <div class="line"></div>
           </div> 
           <!--文字内容-->
           <div class="bulletin">
             <p class="content">{{seller.bulletin}}</p>
           </div>

       </div>
    </div>
    <!--关闭按钮-->
    <div class="detail-close" @click="hideDetail">
      <i class="icon-close"></i>
    </div>
  </div>
  </div> 
 

</template>

<script type="text/ecmascript-6">
// 引入star组件
import star from 'components/star/star.vue';
export default{
  props :{
    seller:{
      type:Object 
    }
  },
  data(){
    return {
      detailShow:false
    }

  },
  methods:{
    showDetail(){
      this.detailShow = true
    },
    hideDetail(){
      this.detailShow = false
    },

  },
  created() {
    this.classMap=["decrease","discount","special","invoice","guarantee"]
  },
  // 注册star组件
  components : {
     star
  }

}

</script>
<style lang="stylus" rel="stylesheet/stylus">
  .clearfix
   display inline-block
   &:after
     display block
     content "."
     height 0
     line-height 0
     clear both
     visibility hidden
  .header
   color :#fff
   background rgba(7,17,27,0.5)
   position relative
   overflow hidden
  .content-wrapper
    padding :24px 12px 18px 24px
    font-size :0
    position relative
    .avatar
     display inline-block
     vertical-align top
     img
      border-radius 2px
    .content
     display inline-block
     margin-left :16px
     font-size :14px
     .title
      margin :2px 0px 8px 0px
      .brand
       display inline-block
       vertical-align :top
       width :30px
       height 18px
       background-image :url("brand@2x.png")
       background-size :cover
       background-repeat :no-repeat
      .name
       margin-left :6px
       font-size :16px
       line-height :18px
       font-weight :bold
    .description
     margin-bottom :10px
     line-height :12px
     font-size :12px
    .supports
      font-size :12px
     .icon
      display inline-block
      vertical-align top
      width 12px
      height 12px
      margin-right 4px
      background-size cover
      background-repeat no-repeat
      &.decrease
       background-image url(decrease_1@2x.png)
      &.discount
       background-image url(discount_1@2x.png)
      &.guarantee
       background-image url(guarantee_1@2x.png)
      &.invoice
       background-image url(invoice_1@2x.png)
      &.special
       background-image url(special_1@2x.png)
     .support-count
     position absolute
     right 12px
     bottom 18px
     padding 0 8px
     height 24px
     line-height 24px      
     border-radius 14px
     background-color rgba(0,0,0,0.2)
     text-align center
    .count
     font-size 12px 
     vertical-align top
    .icon-arrow_lift
     font-size 10px
     line-height 24px
     margin-right 2px
  .bulletin-wrapper
   position relative  
   height 28px
   line-height 28px
   padding 0 22px 0 12px
   white-space nowrap
   overflow hidden
   text-overflow ellipsis
   background rgba(7,17,27,0.2)
   .bulletin-title
     display inline-block
     width 22px
     height 12px
     background-image url("bulletin@2x.png")
     background-size cover
     vertical-align top
     margin-top 7px
    
   .bulletin-text
    font-size 10px
    margin 0 4px
    vertical-align top
   .icon-arrow_lift 
    position absolute
    right 10px
    top 8px
    font-size 10px
  .background
   position absolute
   top 0px
   left 0px
   width 100%
   height 100%
   z-index -1
   filter blur(10px)
  .detail
   position fixed
   top 0
   left 0
   z-index 100
   width 100%
   height 100%
   overflow auto 
   transition all 0.5s //我是注释
   backdrop-filter blur(10px)//ios背景模糊
   &.fade-transition
    opacity 1
    background rgba(0,17,27,0.8)
   &.fade-enter,&.fade-leave
    opacity 0
    background rgba(0,17,27,0)
   .detail-warpper
    min-height 100%
    width 100%
    .detail-main
     margin-top 64px
     padding-bottom 64px
     .name
      line-height 16px
      text-align center
      font-size 16px
      font-weight 800
     .star-wrapper
      margin-top 18px
      padding 2px 0
      text-align center
     .title
      display flex
      width 80%
      margin 28px auto 24px auto
      .line
       flex 1
       position relative
       top -6px
       border-bottom 1px solid rgba(255,255,255,0.2)
      .text
       padding 0 12px
       font-weight 700
       font-size 14px
    .supports
      width 80%
      margin 0 auto
      .supports-item
       padding 0px 12px
       margin-bottom 12px
       font-size 0
       &:last-child
        margin-bottom 0
      .icon
       display inline-block
       width 16px
       height 16px
       background-size cover
       vertical-align top
       margin-right 6px
       &.decrease
        background-image url(decrease_2@2x.png)
       &.discount
        background-image url(discount_2@2x.png)
       &.guarantee
        background-image url(guarantee_2@2x.png)
       &.invoice
        background-image url(invoice_2@2x.png)
       &.special
        background-image url(special_2@2x.png)
      .text
       line-height 16px
       font-size 12px
    .bulletin
      width 80%
      margin 0 auto
      .content
       padding 0px 12px
       line-height 24px
       font-size 12px
       
     
  .detail-close
    position relative
    width 32px
    height 32px
    margin -64px auto 0 auto
    clear both
    font-size 32px
  

</style>



