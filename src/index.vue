<template>
  <div class="wrapper">
    <image :src="logo" class="logo" />
    <text class="greeting">The environment is ready!</text>
    <ButtonItem class="cartIcon" title="购物车"
                  :imgSize= {width:45,height:45} 
                  img="https://s3.cn-north-1.amazonaws.com.cn/h5.taocai.mobi/down/debug.IPA/weexIcon/goodsIcon/shopping_cart2x.png" 
                  :badge='6' 
                  :textCong="{font:22,color:'#999',dotTop:-10,dotRight:20}"></ButtonItem>
  </div>
</template>

<script>
import ButtonItem from './components/ButtonItem'
export default {
  name: 'App',
  components: {
    ButtonItem
  },
  data () {
    return {
      logo: 'https://gw.alicdn.com/tfs/TB1yopEdgoQMeJjy1XaXXcSsFXa-640-302.png'
    }
  }
}
</script>

<style scoped>
  .wrapper {
    justify-content: center;
    align-items: center;
  }
  .logo {
    width: 424px;
    height: 200px;
  }
  .greeting {
    text-align: center;
    margin-top: 70px;
    font-size: 50px;
    color: #41B883;
  }
  .message {
    margin: 30px;
    font-size: 32px;
    color: #727272;
  }
  .cartIcon {
    /* height: 100px; */
    width: 100px;
}
</style>
