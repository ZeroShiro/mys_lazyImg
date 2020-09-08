<template>
  <view class="list_img" :style="{ backgroundColor: bgColor }">
    <image class="img" v-show="!imgError" :class="{ show: showImg }" @load="imageLoad" @error="imageError" :src="url" />
    <view class="err_box" v-if="imgError && errorPath" :src="errorPath">
      <image class="err_img" />
    </view>
  </view>
</template>

<script>
export default {
  props: {
    // 图片地址
    url: {
      type: String,
    },
    bgColor: {
      type: String,
    },
    errorPath: {
      type: [String, Boolean],
      default: "/static/images/loadfail.png",
    },
  },

  data() {
    return {
      showImg: false,
      imgError: false,
    };
  },
  methods: {
    imageLoad(e) {
      this.imgError = false;
      setTimeout(() => {
        this.showImg = true;
      }, 50);
    },
    imageError(e) {
      this.imgError = true;
      console.log("imgErr");
    },
  },
};
</script>

<style lang="scss" scoped>
.list_img {
  width: 100%;
  height: 100%;
  background-color: #f4f5f5;
  border-radius: 8rpx;
  position: relative;
  .img {
    width: 100%;
    height: 100%;
    opacity: 0;
    transition: opacity 0.5s ease-in;
    border-radius: 8rpx;
    &.show {
      opacity: 1;
    }
  }
}
.err_box {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;

  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  .err_img {
    opacity: 1;
    width: 120rpx;
    height: 120rpx;
  }
}
</style>
