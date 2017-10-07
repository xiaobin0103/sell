<template>
  <div class="star" :class="starType">
      <span v-for="itemClass in itemClasses" track-by="$index" :class="itemClass" class="star-item" ></span>

  </div>
</template>

<script>
  // 常量
  const LENGTH =5;
  const CLS_ON="on"
  const CLS_HALF="half"
  const CLS_OFF="off"
	export default {
  	props : {
		  size: {
        type:Number

		  },
      score: {
        type:Number

		  },
	  },
    computed : {
      starType(){
        return "star-"+this.size
      },
      itemClasses(){
        let result=[]
       
        let score=Math.floor(this.score * 2)/2
       
        // 如果有小数
        let hasDecimal = score % 1 !== 0
        // 如果是整数
        let integer = Math.floor(score)
        
       
        // 全星      
        for (let i = 0; i < integer; i++) {
         
          result.push(CLS_ON)
          
                  
        }
        // 半星
        if (hasDecimal) {
           result.push(CLS_HALF)
        }
        // 关闭的星
        while(result.length<LENGTH){
          result.push(CLS_OFF)
        }
        return result

      }

    }

}
</script>

<style lang="stylus" rel="stylesheet/stylus">
 .star
  font-size 0
  .star-item
   display inline-block
   background-repeat no-repeat
  &.star-48
   .star-item
    width 20px
    height 20px
    margin-right 22px
    background-size cover
    &:last-child
     margin-right 0
    &.on
     background-image url("star48_on@2x.png")
    &.half
     background-image url("star48_half@2x.png")
    &.off
     background-image url("star48_off@2x.png")
  &.star-36
   .star-item
    width 15px
    height 15px
    margin-right 6px
    background-size cover
    &:last-child
     margin-right 0
    &.on
     background-image url("star36_on@2x.png")
    &.half
     background-image url("star36_half@2x.png")
    &.off
     background-image url("star36_off@2x.png")
  &.star-24
   .star-item
    width 10px
    height 10px
    margin-right 3px
    background-size cover
    &:last-child
     margin-right 0
    &.on
     background-image url("star24_on@2x.png")
    &.half
     background-image url("star24_half@2x.png")
    &.off
     background-image url("star24_off@2x.png")
</style>