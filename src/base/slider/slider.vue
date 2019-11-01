<template>
  <div class="slider" ref="slider">
    <div class="slider-group" ref="sliderGroup">
      <slot></slot>
    </div>
    <div class="dots">
      <span class="dot" v-for="(item, index) in dots" :key="index" :class="{active:index === currentPageIndex}"></span>
    </div>
  </div>
</template>
<script>
import { addClass } from "common/js/dom";
import BScroll from "better-scroll";
export default {
  data() {
    return {
      dots:[],
      currentPageIndex: 0
    };
  },
  props: {
    loop: {
      type: Boolean,
      default: true
    },
    autoPlay: {
      type: Boolean,
      default: true
    },
    interval: {
      type: Number,
      default: 4000
    }
  },
  mounted() {
    setTimeout(() => {
      this._setSliderWidth();
      this._initDots();
      this._initSlider();
    }, 20);
  },
  methods: {
    _setSliderWidth() {
      this.children = this.$refs.sliderGroup.children;
      let width = 0;
      let sliderWidth = this.$refs.slider.clientWidth;
      for (let i = 0; i < this.children.length; i++) {
        let child = this.children[i];
        addClass(child, "slider-item");
        child.style.width = sliderWidth + "px";
        width += sliderWidth;
      }
      if (this.loop) {
        width += 2 * sliderWidth;
      }
      this.$refs.sliderGroup.style.width = width + "px";
    },
    _initDots() {
      this.dots = new Array(this.children.length);
    },
    _initSlider() {
      this.slider = new BScroll(this.$refs.slider, {
        scrollX: true,
        scrollY: false,
        momentum: false, //滚动动画
        snap: {
          loop: this.loop,
          threshold: 0.3,
          speed: 400
        },
        snapSpeed: 400,
        click: true
      });
      
    }
  }
};
</script>
<style lang="stylus" scoped>
@import '~common/stylus/variable';

.slider {
  min-height: 1px;

  .slider-group {
    position: relative;
    overflow: hidden;
    white-space: nowrap;

    .slider-item {
      float: left;
      box-sizing: border-box; // width将包含内容 padding 和 border
      overflow: hidden;
      text-align: center;

      a {
        display: block;
        width: 100%;
        overflow: hidden;
        text-decoration: none;

        img {
          width: 100%; // 在img中只设置宽度不设置高度就会等比缩放
          display: block;
        }
      }
    }
  }

  .dots {
    position: absolute;
    right: 0;
    left: 0;
    bottom: 12px;
    font-size: 0;
    text-align: center;

    .dot {
      display: inline-block;
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background-color: $color-text-l;
      margin: 0 4px;

      &.active {
        background-color: $color-text-ll;
        width: 20px;
        border-radius: 5px;
      }
    }
  }
}
</style>