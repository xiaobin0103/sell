<template>
  <div class="goods">
    <!--左侧菜单-->
    <div class="menu-wrapper" v-el:menu-wrapper>
      <ul>
        <li v-for="item in goods"  class="menu-item" :class="{'current':currentIndex===$index}" @click="selectMenu($index,$event)">
          <span class="text">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}</span>
        </li>
      </ul>

    </div>
    <!-- 食品列表 -->
    <div class="foods-wrapper"  v-el:foods-wrapper>
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <!-- 点击商品进入详情 -->
            <li @click="selectFood(food,$event)" v-for="food in item.foods" class="food-item">
              <div class="icon">
                <img width="57" height="57" :src="food.icon" alt="">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                <span class="count">月售{{food.sellCount}}份</span>
                <span>好评率{{food.rating}}%</span>
              </div>
              <!-- 降价信息 -->
              <div class="price">
                <span class="now">¥{{food.price}}</span>
                <span class="old" v-show="food.oldPrice">{{food.oldPrice}}</span>
              </div>
              <!-- 购物按钮 -->
              <div class="cartconcontrol-wrapper">
                <cartconcontrol :food="food"></cartconcontrol>  
              </div>
              
              </div>
             
            </li>
          </ul>
        </li>
      </ul>
    
    
    </div>
    <!-- 购物车组件 传递配送费 起送价-->
    <shopcart v-ref:shopcart :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice" :select-foods="selectFoods"></shopcart> 
    <!-- 商品详情组件 -->
    <food :food="selectedFood" v-ref:food transition="move"></food>
  </div>

</template>

<script type="text/ecmascript-6">
  import BScroll from "better-scroll"
  //引入购物车组件
  import shopcart from "components/shopcart/shopcart" 
  //引入购物按钮组件 
  import cartconcontrol from "components/cartconcontrol/cartconcontrol"
  // 商品详情组件
  import food from "components/food/food"
  const ERR_OK = 0
  export default{
    props: {
      seller: {
        type : Object
      }
    },
    data (){
      return {
        goods: [],
        listHeight:[],
        scrollY:0,
        selectedFood:{}
      }
    },
    computed: {
      currentIndex(){
         for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];

          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
        return 0;
      },
      selectFoods(){
        let foods=[]
        this.goods.forEach((good)=>{
          good.foods.forEach((food)=>{
            if (food.count) {
              foods.push(food)

              
            }
          })

        })
        return foods
      }
    },
    created() {
       this.classMap=["decrease","discount","special","invoice","guarantee"]
       this.$http.get("./api/goods").then((response)=>{
            response = response.body
            if(response.errno === ERR_OK ){
              this.goods= response.data
              this.$nextTick(() => {
                this._initScroll()
               
                this._calculateHeigth()
              })
              
            }
       })
    },
    //初始化BSscroll
    methods:{
      // 商品详情
      selectFood(food,event){
          if(!event._constructed){
          return
        } 
        this.selectedFood = food
        this.$refs.food.show()
      },
      //点击选择 
      selectMenu(index,event){      
        //如果是原生事件 阻止 
        if(!event._constructed){
          return
        }  
           
        let foodList = this.$els.foodsWrapper.getElementsByClassName("food-list-hook")
        let el=foodList[index]
       
        this.foodsScroll.scrollToElement(el,300)

      },
      _initScroll(){
          this.menuScroll = new BScroll(this.$els.menuWrapper,{
            click:true
          }) 
          this.foodsScroll = new BScroll(this.$els.foodsWrapper,{
            probeType:3,
            click:true
          }) 
          this.foodsScroll.on("scroll",(pos)=>{
            this.scrollY=Math.abs(Math.round(pos.y))
            
          })
      },
      _calculateHeigth(){
        //获取原生dom 
        let foodList = this.$els.foodsWrapper.getElementsByClassName("food-list-hook")
        let height=0
        this.listHeight.push(height)
        for (let i = 0; i < foodList.length; i++) {
             let item = foodList[i];
          
            height += item.clientHeight;
            this.listHeight.push(height);
          
        }
       

        
      },
      // 小球动画
      _drop(target){
          this.$refs.shopcart.drop(target)
      }
    },
    //注册组件
    components: {
      shopcart,
      cartconcontrol,
      food
    }, 
    // 接收事件
    events: {
      "cart.add"(target){
          this._drop(target)
      }
    }

  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  .goods
   display flex
   position absolute
   top 174px
   bottom 46px
   width 100%
   overflow hidden
   .menu-wrapper
    flex 0 0 80px 
    width 80px
    background #f3f5f7
    .menu-item
     display table
     height 54px
     width 56px
     line-height 14px
     padding 0 12px
     &.current
      position relative
      margin-top -1px
      z-index 10
      background #ffffff
      font-weight 700
      .text
       border none
     .icon
      display inline-block
      vertical-align top
      width 12px
      height 12px
      margin-right 2px
      background-size cover
      background-repeat no-repeat
      &.decrease
       background-image url(decrease_3@2x.png)
      &.discount
       background-image url(discount_3@2x.png)
      &.guarantee
       background-image url(guarantee_3@2x.png)
      &.invoice
       background-image url(invoice_3@2x.png)
      &.special
       background-image url(special_3@2x.png)
    .text
     display table-cell
     font-size 12px 
     width 56px
     vertical-align middle
     border-bottom 1px solid rgba(7,17,27,0.1)

   .foods-wrapper
    flex 1
    .title
     padding-left 14px
     height 26px
     line-height 26px
     border-left 2px solid #d9dde1
     font-size 12px
     color rgb(143,153,159)
     background #f3f5f7
    .food-item
     display flex
     margin 18px
     padding-bottom 18px
     border-bottom 1px solid rgba(7,17,27,0.1)
     position relative
     &:last-child
      border none
      margin-bottom 0
     .icon
      flex 0 0 57px
      margin-right 10px
     .content
      flex 1
      .name
       margin 2px 0 8px 0
       height 14px
       line-height 14px
       font-size 14px
       color rgb(7,17,27)
      .desc
       margin-bottom 8px
       line-height 12px
       font-size 10px
       color rgb(143,153,159)
      .extra
       line-height 10px
       font-size 10px
       color rgb(143,153,159)
       .count
        margin-right 12px
      .price
       font-weight 700
       line-height 24px
       .now
        margin-right 8px
        font-size 14px
        color rgb(240,20,20)
      .old
       text-decoration line-through
       font-size 10px
       color rgb(143,153,159)
      .cartconcontrol-wrapper
       position absolute
       right 0
       bottom 12px
        



</style>
