<template>
      <div class="ldy-tabs-view" :class="{'ldy-tabs-fixed':isFixed,'ldy-tabs-relative':!isFixed,'ldy-unlined':unlined}" :style="{height:height+'rpx',paddingLeft:padding+'rpx',paddingRight:padding+'rpx',backgroundColor:backgroundColor,top:isFixed?top+'px':'auto'}">
        <div v-for="(item,index) in tabs" :key="index" class="ldy-tabs-item" :style="{width:itemWidth}" @click="swichTabs(index)">
            <div class="ldy-tabs-title" :class="{'ldy-tabs-active':currentTab==index,'ldy-tabs-disabled':item.disabled}" :style="{color:currentTab==index?selectedColor:color,fontSize:size+'rpx',lineHeight:size+'rpx',fontWeight:bold && currentTab==index?'bold':'normal'}">{{item.name}}</div>
        </div>
        <div class="ldy-tabs-slider" :style="styleObject"></div>
    </div>
</template>

<script>
export default {
  name: '',
  props:{
          //标签页
    tabs: {
      type: Array,
      default: []
    },
    //rpx
    height: {
      type: Number,
      default: 80
    },
    //rpx 只对左右padding起作用，上下为0
    padding: {
      type: Number,
      default: 30
    },
    //背景色
    backgroundColor: {
      type: String,
      default: "#FFFFFF"
    },
    //是否固定
    isFixed: {
      type: Boolean,
      default: false
    },
    //px
    top: {
      type: Number,
      default: 0
    },
    //是否去掉底部线条
    unlined: {
      type: Boolean,
      default: false
    },
    //当前选项卡
    currentTab: {
      type: Number,
      default: 0
    },
    //滑块宽度
    sliderWidth: {
      type: Number,
      default: 68
    },
    //滑块高度
    sliderHeight: {
      type: Number,
      default: 6
    },
    //滑块背景颜色
    sliderBgColor: {
      type: String,
      default: "#5677fc"
    },
    sliderRadius: {
      type: String,
      default: "50rpx"
    },
    //滑块bottom
    bottom: {
      type: String,
      default: "0"
    },
    //标签页宽度
    itemWidth: {
      type: String,
      default: "25%"
    },
    //字体颜色
    color: {
      type: String,
      default: "#666"
    },
    //选中后字体颜色
    selectedColor: {
      type: String,
      default: "#5677fc"
    },
    //字体大小
    size: {
      type: Number,
      default: 28
    },
    //选中后 是否加粗 ，未选中则无效
    bold: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
        winWidth: 0,
        scrollLeft: 0
    };
  },
  computed:{
    styleObject(){
        return this.styles({
            'transform':`translateX(${this.scrollLeft}px)`,
            'width':this.sliderWidth+'rpx',
            'height':this.sliderHeight+'rpx',
            'borderRadius':this.sliderRadius,
            'bottom':this.bottom,
            'backgroundColor':this.sliderBgColor,
            'marginBottom':this.bottom=='50%'?('-'+this.sliderHeight/2+'rpx'):0
        })
    }
  },
  watch:{
      currentTab(){
          this.checkCor();
      },
      tabs(){
          this.checkCor();
      }
  },
  methods: {
    checkCor() {
      let tabsNum = this.tabs.length
      let padding = this.winWidth / 750 * this.padding
      let width = this.winWidth - padding * 2
      let left = (width / tabsNum - (this.winWidth / 750 * this.sliderWidth)) / 2 + padding
      let scrollLeft = left
      if (this.currentTab > 0) {
        scrollLeft = scrollLeft + (width / tabsNum) * this.currentTab
      }
      this.scrollLeft = scrollLeft
    },
    // 点击标题切换当前页时改变样式
    swichTabs(index) {
      let item = this.tabs[index]
      if (item && item.disabled) return;
      if (this.currentTab == index) {
        return false;
      } else {
        this.$emit("change", index)
      }
    },
    /*
    *  将对象转变为class字符串
    */
    classes(obj) {
        let classesStr = '';
        for (let className in obj) {
            if (obj.hasOwnProperty(className) && !!obj[className]) classesStr += className;
        }
        return classesStr;
    },
    //将对象转变为style字符串，对象属性的camel命名会转换为中划线命名
    styles(obj) {
        let stylesStr = '';
        for (let styleName in obj) {
            if (obj.hasOwnProperty(styleName)) stylesStr += `${styleName.replace(/([A-Z])/g, "-$1").toLowerCase()}:${this.px2rpx(obj[styleName])};`;
        }
        return stylesStr;
    },
    px2rpx(str) {
        if (/rpx/.test(str)) return str
        else if (/px/.test(str) && !/translateX/.test(str)) {
            let val = str.substring(0, str.indexOf('px'))
            /*在代码中硬编码时，所有px单位都是以375px为整个屏幕宽度作为参照，所以这里需要转换一下*/
            val = val * this.winWidth / 375
            return `${(val - 0) * 750 / this.winWidth}rpx`
        } else return str
    }
  },
    onLoad(){
        setTimeout(() => {
            wx.getSystemInfo({
                success: (res) => {
                    this.winWidth = res.windowWidth;
                    this.checkCor();
                }
            });
        }, 10);
    },
    onUnload(){
    }
};
</script>

<style scoped lang="less">
.ldy-tabs-view {
		width: 100%;
		box-sizing: border-box;
		display: flex;
		align-items: center;
		justify-content: space-between;
		z-index: 996;
	}

	.ldy-tabs-relative {
		position: relative;
	}

	.ldy-tabs-fixed {
		position: fixed;
		left: 0;
	}

	.ldy-tabs-fixed,
	.ldy-tabs-relative {
        &::before{
            content: '';
            position: absolute;
            border-bottom: 1rpx solid #eaeef1;
            -webkit-transform: scaleY(0.5) translateZ(0);
            transform: scaleY(0.5) translateZ(0);
            transform-origin: 0 100%;
            bottom: 0;
            right: 0;
            left: 0;
        }
	}

	.ldy-unlined {
        &::before{
            border-bottom: 0 !important
        }
	}

	.ldy-tabs-item {
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.ldy-tabs-disabled {
		opacity: .6;
	}

	.ldy-tabs-title {
		display: flex;
		align-items: center;
		justify-content: center;
		position: relative;
		z-index: 2;
	}

	.ldy-tabs-active {
		transition: all 0.15s ease-in-out;
	}

	.ldy-tabs-slider {
		position: absolute;
		left: 0;
		transition: all 0.15s ease-in-out;
		z-index: 0;
		transform: translateZ(0);
	}
</style>
