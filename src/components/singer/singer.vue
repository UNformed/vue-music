<template>
  <div class="singer">
    <listview :data="singers"></listview>
  </div>
</template>
<script>
import { getSingerList } from "api/singer";
import { ERR_OK } from "api/config";
import Singer from "common/js/singer";
import Listview from "base/listview/listview"
const HOT_NAME = "热门";
const HOT_SINGER_LEN = 10;
export default {
  data() {
    return {
      singers: []
    };
  },
  components: {
    Listview
  },
  created() {
    this._getSingerList();
  },
  methods: {
    _getSingerList() {
      getSingerList().then(res => {
        if (res.code === ERR_OK) {
          this.singers = this._normalizeSinger(res.data.list)
        }
      });
    },
    _normalizeSinger(list) {
      let map = {
        hot: {
          title: HOT_NAME,
          items: []
        }
      };
      list.forEach((item, index) => {
        if (index < HOT_SINGER_LEN) {
          map.hot.items.push(
            new Singer({ id: item.Fsinger_mid, name: item.Fsinger_name })
          );
        }
        const key = item.Findex;
        if (!map[key]) {
          map[key] = {
            title: item.Findex,
            items: []
          };
        }
        map[key].items.push(
          new Singer({ id: item.Fsinger_mid, name: item.Fsinger_name })
        );
      });
      let hot = [];
      let ret = [];
      for (const key in map) {
        let val = map[key];
        if (val.title.match(/[a-zA-Z]/)) {
          ret.push(val);
        } else if (val.title === HOT_NAME) {
          hot.push(val);
        }
      }
      ret.sort((a, b) => {
        return a.title.charCodeAt(0) - b.title.charCodeAt(0);
      });
      return hot.concat(ret);
    }
  }
};
</script>
<style lang="stylus" scoped rel="stylesheet/stylus">
.singer {
  position: fixed; // 绝对定位，相对于浏览器窗口
  top: 88px;
  bottom: 0;
  width: 100%;
}
</style>