<!-- 自定义button组件ButtonItem  -->
<template>
    <div class="button-item buttonshadow" 
        ref="button_item" 
        :style="bindBtnType()" @click="onclick">
        <image v-if="img.length > 0" :src='img' 
            :style="{ width:imgSize.width + 'px', height:imgSize.height + 'px'}"/>
        <text v-if="title.length > 0" class="button-title" 
              :style="{ fontSize:textCong.font + 'px', color:textCong.color }">{{ title }}</text>
        <div class="badge-tag" v-if="badge > 0" 
            :style="{right:(textCong.dotRight==10 || !textCong.dotRight)?10:textCong.dotRight, 
                     top:(textCong.dotTop==10 || !textCong.dotTop)?10:textCong.dotTop,
                     'padding-left':isWeb?10:8,'padding-right':isWeb?10:8}">
          <text class="badge-text">{{badge}}</text>
        </div>
        <div v-if="dot && badge<=0" class="dot" 
            :style="{right:(textCong.dotRight==0 ||!textCong.dotRight)?0:textCong.dotRight,
                     top:(textCong.dotTop==10 || !textCong.dotTop)?10:textCong.dotTop}"></div>
    </div>
</template>
<script>

export default {
    props:{
        BtnType: {
            type: Number,
            default: 0 //0:上图下字;1:左图右字
        },
        img: {
            type: String,//图片URL
            default: 'https://gw.alicdn.com/tfs/TB1ARoKSXXXXXc9XVXXXXXXXXXX-72-72.png'
        },
        imgSize: {
            type:Object,//图片尺寸
            default:()=>({
                width:60,
                height:60
            })
        },
        title: {
            type: String,
            default: '确认'
        },
        textCong: {
            type:Object,
            default:()=>({
               font:34, //文字大小
               color:'#333',//文字颜色
               dotRight:10, //红点或角标到右边的距离
               dotTop:10 //红点或角标到上边的距离
            })
        },
        badge: {
            type: Number,
            default: 0 
        },
        dot: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            isWeb : WXEnvironment.platform === "Web",
        }
    },
    created () {

    },
    methods: {
        //button布局类型
        bindBtnType:function(){
            var that = this;
            let directionType = {};
            if(that.BtnType == 0){
                directionType = {
                     "flex-direction":"column",
                } 
            }else if(that.BtnType == 1) {
                directionType = {
                     "flex-direction":"row",
                } 
            }
            return directionType;
        },
        //点击事件
        onclick:function(e) {
            this.$emit('onclick', e);
        }
    }
}
</script>
<style scoped>
  .button-item {
    justify-content: center;
    align-items: center;
    border-bottom-style: solid;
    /* overflow: visible; */
    background-color: aqua; 
  }
  .buttonshadow{
      box-shadow: inset 0 0px 0px rgba(0, 0, 0, 0);
  }
  .buttonshadow :active{
      box-shadow: inset 0 15px 40px rgba(0, 0, 0, 0.2);
  }
  
  .button-title {
    lines: 1;
    text-overflow: ellipsis;
    text-align: center;
  }
.badge-tag {
    position: absolute;
    top: 10px;
    right: 10px;
    border-radius: 15px;
    background-color: red;
    height: 30px;
    align-items: center;
    justify-content: center;
    padding-left: 8px;
    padding-right: 8px;
}
.badge-text {
    lines: 1;
    font-size: 20px;
    color: #ffffff;
}
.dot {
    width: 20px;
    height: 20px;
    border-bottom-right-radius: 20px;
    border-bottom-left-radius: 20px;
    border-top-left-radius: 20px;
    border-top-right-radius: 20px;
    position: absolute;
    top: 10px;
    right: 0px;
    background-color: red;
}
</style>
