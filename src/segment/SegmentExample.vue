<!-- 实例demo  -->
<template>
    <div style="flex:1;">
        <Segmented class="segment" 
                   ref="segment" 
                   :items='array'
                   itemWidth=200
                   tintColor='red'
                   @changedClick="onChanged"></Segmented>
        <scroller  class="scroller"
                   ref="scroller"
                   scroll-direction="horizontal"
                   :show-scrollbar="false"  
                   pagingEnabled=true
                   @scroll="onDidScroll" 
                   @scrollstart="scrollStart" 
                   @scrollend="scrollEnd">
            <div v-for="(item, index) in data" :key="index" :ref="'list-'+index">
                <ClassList class="list" :classId='item.id'></ClassList>
                <!-- <div v-if="item.id==0">
                    <homepage></homepage>
                </div>
                <div v-else>
                    <scroller class="list" :ref="'list-'+index">
                        <ClassList :classId='item.id'></ClassList>
                    </scroller>
                </div> -->
            </div>
        </scroller>
    </div>
</template>
<script>
import Segmented from "@/segment/Segmented.vue";
import ClassList from "@/segment/ClassList.vue";
import Fetch from '@/Fetch.js'

const modal = weex.requireModule("modal");
const dom = weex.requireModule('dom')

export default {
    components: { Segmented,  ClassList },
    name: "实例",
    data() {
        return {
            //控制手势滑动
            dragging:false,
            cutentPage:0,
            //设置选中界面是否动画
            scrollAnimated:false,
            array:['推荐1','水果','蔬菜','海鲜','干货','肉禽蛋奶',],
            //服务器返回数据
            data:[]
        }
    },

    created() {
        if(this.cutentPage == 0){
            this.scrollAnimated = true;
        }
        //加载tab分类
        this.loadTabData();
    },
    mounted: function () {
    },
    methods: {
        //加载tab分类数据
        loadTabData: function(){
            var that = this;

            Fetch.post('superior/wechat/layoutConfig.json',{},function(jsonString) {
                // console.log('post:'+jsonString);
                that.data = JSON.parse(jsonString).classList;
                
                var titleArray = [];
                that.data.forEach(function(item,index){
                   let className = item.className;
                   if(className){
                        titleArray.push(className);
                    } 
                })
                that.array = titleArray;

            },function(err){ 
                console.log('err:'+err);
            })
        },
        //标签点击事件
        onChanged:function(title) {
            var that = this;
            console.log(title);
            let index = that.array.indexOf(title);
            that.cutentPage = index;
            
            const el = that.$refs['list-'+index][0];
            console.log('执行滑动事件外面',el)
            if(el){
                console.log('执行滑动事件里面')
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
    /* background-color: yellow; */
}
.list {
    width: 750px;
    flex:1;
}
</style>
