<template>
  <scroll class="listview" :data="data">
    <ul>
      <li class="list-group" v-for="(group,index) in data" :key="index">
        <h2 class="list-group-title">{{group.title}}</h2>
        <ul>
          <li class="list-group-item" v-for="(item,indexes) in group.items" :key="indexes">
            <img v-lazy="item.avatar" alt class="avatar" />
            <span class="name">{{item.name}}</span>
          </li>
        </ul>
      </li>
    </ul>
  </scroll>
</template>
<script>
import Scroll from "base/scroll/scroll";
export default {
  data() {
    return {};
  },
  props: {
    data: {
      type: Array,
      default: []
    }
  },
  components: {
    Scroll
  },
  mounted() {
    console.log(this.data);
  }
};
</script>
<style lang="stylus" scoped>
@import '~common/stylus/variable';

.listview {
  height: 100%;
  width: 100%;
  position: relative;
  background-color: $color-background;
  overflow hidden
  .list-group {
    width: 100%;
    padding-bottom: 30px;

    .list-group-title {
      color: $color-text-l;
      background: $color-highlight-background;
      height: 30px;
      line-height: 30px;
      padding-left: 20px;
      font-size: $font-size-small;
    }

    .list-group-item {
      display: flex;
      align-items: center;
      padding: 20px 0 0 30px;

      .avatar {
        width: 50px;
        height: 50px;
        border-radius: 50%;
      }

      .name {
        margin-left: 20px;
        color: $color-text-l;
        font-size: $font-size-medium;
      }
    }
  }
}
</style>