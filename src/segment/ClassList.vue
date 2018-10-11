<!-- 分类数据  -->
<template>
<div>
    <scroller alwaysScrollableVertical='true'>
        <div class="content">
            <div v-for="(model,i) in lists" :key="i">
                <div v-if="model.bigImage =='true'">
                    <div class="bigcell" @click="cellClick(model)">
                        <image class="bigImg" resize="stretch" :src="model.goodsImage"/>
                        <div class="textcon">
                            <div class="text-left">
                                <text class="textName" style="width: 420px;">{{model.supGoodsName}}</text>
                                <text class="textDes" style="width: 420px;">{{model.supGoodsDescribe}}</text>
                            </div>
                            <div class="text-right">
                                <text style="font-size: 36px;color: #ff0033">$</text>
                                <text style="font-size: 36px;color: #ff0033">{{model.storePrice}}</text>
                            </div>
                        </div>
                    </div> 
                </div>
                <div v-else>
                    <div class="smallcell" @click="cellClick(model)"> 
                        <image class="smallImg" resize="stretch" :src="model.goodsImage"/>
                        <text class="textName">{{model.supGoodsName}}</text>
                        <text class="textDes">{{model.supGoodsDescribe}}</text>
                        <div class="text-right">
                            <text style="font-size: 30px;color: #ff0033">$</text>
                            <text style="font-size: 34px;color: #ff0033">{{model.storePrice}}</text>
                        </div>
                    </div>
                </div> 
            </div>
        </div>
    </scroller>
</div>
</template>

<style scoped>
.content {
    width: 750px;
    flex-direction: row;
    flex-wrap: wrap;
    /* justify-content: flex-start; */
    /* align-items: center; */
}
.bigImg{
    margin-left: 20px;
    width: 710px;
    height: 400px;
    background-color:#ddd
}
.smallImg{
    width: 335px;
    height: 240px;
    margin-left: 20px;
    margin-right: 20px;
    background-color:#ddd
}
.textcon{
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    margin-left: 20px;
    margin-right: 60px;
}
.text-right{
    flex-direction: row;
    align-items: center;
    margin-left: 20px;
}
.textName{
    lines: 1;
    text-overflow: ellipsis;
    font-size: 34px;
    width: 335px;
    margin-left: 20px;
    margin-right: 20px;
    color: #333 
}
.textDes {
    lines: 1;
    text-overflow: ellipsis;
    font-size: 28px;
    width: 335px;
    margin-left: 20px;
    margin-right: 20px;
    color: #666 
}
</style>

<script>
import Fetch from '@/Fetch.js'

const modal = weex.requireModule("modal");

export default {
    //定义属性
    props: {
        classId: {
            default:5
        },
    },
     data() {
         return {
             page:1,
             lists: []
         }
     },
     created() {
         this.loadData();
     },
     mounted: function () {
         
     },
     methods: {
          //加载数据
          loadData: function() {
              var that = this;
              var param = {'id': that.classId,'currentPage':that.page}
              let uri = 'superiorGoods/byClass.json';
              
              Fetch.post(uri, param, function(jsonStr){
                //   console.log('byClass:'+jsonStr);
                  that.lists = JSON.parse(jsonStr).goodsList;
                  
              },function(err){
                  console.log('err:'+err);
              })
          },
          //点击事件
          cellClick:function(model){
              modal.toast({
                  message:model.supGoodsName,
                  duration: 0.3
                  });
          },
     }

}
</script>
