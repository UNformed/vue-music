<template>
  <div class="recommend">
    <scroll class="recommend-content" :data="discList">
      <div>
        <div v-if="recommends.length" class="slider-wrapper">
          <slider>
            <div v-for="(item, index) in recommends" :key="index">
              <a :href="item.linkUrl">
                <img :src="item.picUrl" alt />
              </a>
            </div>
          </slider>
        </div>
        <div class="recommend-list">
          <h1 class="list-title">热门歌单推荐</h1>
          <ul>
            <li v-for="(item,index) in discList" :key="index" class="item">
              <div class="icon">
                <img :src="item.imgurl" width="60" height="60" />
              </div>
              <div class="text">
                <h2 class="name" v-html="item.creator.name"></h2>
                <p class="desc" v-html="item.dissname"></p>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </scroll>
  </div>
</template>
<script>
import { getRecommend, getDiscList } from "api/recommend";
import { ERR_OK } from "api/config";
import Slider from "base/slider/slider";
import Scroll from "base/scroll/scroll"
export default {
  data() {
    return {
      recommends: [],
      discList: []
    };
  },
  created() {
    this._getRecommend();
    this._getDiscList();
  },
  components: {
    Slider,
    Scroll
  },
  methods: {
    _getRecommend() {
      getRecommend().then(res => {
        if (res.code === ERR_OK) {
          this.recommends = res.data.slider;
        }
      });
    },
    _getDiscList() {
      getDiscList().then(res => {
        if (res.code === ERR_OK) {
          this.discList = res.data.list;
        }
      });
    }
  }
};
</script>
<style lang="stylus" scoped>
@import '~common/stylus/variable';

.recommend {
  position: fixed;
  width: 100%;
  top: 88px;
  bottom: 0;

  .recommend-content {
    height: 100%;
    overflow: hidden;

    .slider-wrapper {
      position: relative;
      width: 100%;
      overflow: hidden;
    }

    .recommend-list {
      .list-title {
        height: 65px;
        line-height: 65px;
        color: $color-theme;
        font-size: $font-size-medium;
        text-align: center;
      }

      .item {
        display: flex;
        align-items: center;
        box-sizing: border-box; // width将包含padding和border
        padding: 0 20px 20px 20px;

        .icon {
          width: 60px;
          flex: 0 0 60px; // 有剩余空间也不放大，空间不足也不缩小，宽度为60
          padding-right: 20px;
        }

        .text {
          display: flex;
          flex-direction: column;
          justify-content: center;
          flex: 1;
          line-height: 20px;
          overflow: hidden;
          font-size: $font-size-medium;

          .name {
            margin-bottom: 10px;
            color: $color-text;
          }

          .desc {
            color: $color-text-d;
          }
        }
      }
    }
  }
}
</style>