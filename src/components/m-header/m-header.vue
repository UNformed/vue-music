<template>
  <div class="m-header">
    <div class="icon"></div>
    <h1 class="text">Chicken Music</h1>
  </div>
</template>
<script>
export default {};
</script>
<style lang="stylus" scoped>
@import '~common/stylus/variable';
@import '~common/stylus/mixin';

.m-header {
  position: relative;
  text-align: center;
  color: $color-theme;
  height: 40px;
  font-size: 0;

  .icon {
    bg-image('logo');
    width: 30px;
    height: 32px;
    background-size: 30px 32px;
    margin-right: 9px;
    vertical-align: top;
    margin-top: 6px;
    display: inline-block;
  }

  .text {
    display: inline-block;
    vertical-align: top;
    line-height: 44px;
    font-size: $font-size-large;
  }
}
</style>