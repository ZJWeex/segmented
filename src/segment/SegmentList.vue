<!-- 列表页  -->
<template>
    <div style="flex:1;background-color:skyblue">
        <Segmented class="segment" 
                   ref="segment" 
                   :items='array'
                   itemWidth=200
                   tintColor='red'
                   @changedClick="onChanged"></Segmented>
        <scroller  class="scroller"
                   ref="scroller"
                   scroll-direction="horizontal" 
                   pagingEnabled=true
                   @scroll="onDidScroll" 
                   @scrollstart="scrollStart" 
                   @scrollend="scrollEnd">
            <div v-for="(item, index) in array" :key="index">
                <div class="list" :style="randColor()"  :ref="'list-'+index">
                    <text>{{item}}</text>
                </div>
            </div>
        </scroller>
    </div>
</template>
<script>
import Segmented from "@/segment/Segmented.vue";

const modal = weex.requireModule("modal");
const dom = weex.requireModule('dom')

export default {
    components: { Segmented },
    name: "首页",
    data() {
        return {
            //控制手势滑动
            dragging:false,
            cutentPage:1,
            //设置选中界面是否动画
            scrollAnimated:false,
            array:['推荐','水果','蔬菜','海鲜','干货','肉禽蛋奶',]
        }
    },

    created() {
    },
    mounted: function () {
     //设置某个选中 
     this.$refs['segment'].setSelectIndex(this.cutentPage) 
    },
    methods: {
        //标签点击事件
        onChanged:function(title) {
            var that = this;
            console.log(title);
            let index = that.array.indexOf(title);
            that.cutentPage = index;
            const el = that.$refs['list-'+index][0];
            if(el){
                console.log(index);
                dom.scrollToElement(el, { offset: 0 ,animated: that.scrollAnimated})
                that.scrollAnimated = true;
            }else{
                console.log('没有找到元素ref=scroller')
            }
            
        },
        //Scroll滑动事件
        scrollStart:function(event) {
            console.log('开始');
            this.dragging = true;
        },
        onDidScroll:function(event) {
            if(this.dragging == true){
                console.log('......');
                let offsetX = event.contentOffset.x;
                let page = Math.abs(Math.round(offsetX/750.0));
                if(this.cutentPage != page) {
                    this.cutentPage = page;
                    this.$refs['segment'].setSelectIndex(this.cutentPage)
                }  
            }
        },
        scrollEnd:function(event) {
            console.log('结束');
            this.dragging = false;
        },
        //生成随机颜色值
        randColor: function() {
            let colorStr = '#'+Math.floor(Math.random()*0xffffff).toString(16);
            console.log(colorStr);
             return {'background-color': colorStr}
        },
    }

}
</script>

<style scoped>
.scroller {
    flex:1;
    background-color: yellow;
}
.list {
    width: 750px;
    flex:1;
    justify-content: center;
    align-items: center;
}
</style>
