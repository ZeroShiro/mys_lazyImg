<template>
  <view class="mysImg_box" :id="boxId" :style="{ height: imgH, width: imgW, borderRadius: bRadius, backgroundColor: boxColor }">
    <image
      class="mys_Img"
      v-if="isLoad"
      @load="mLoad"
      :mode="imgMode"
      @error="imageError"
      :data-id="imgId || boxId"
      v-show="showImg"
      :class="{ mTransition: resultImg }"
      :style="{ height: imgH, width: imgW, borderRadius: bRadius, transitionDuration: showDuration }"
      :src="imgUrl"
    ></image>

    <!-- 加载中 效果 -->
    <view
      :class="[loadMode, 'loading']"
      v-if="loadMode && !resultImg && !imgError"
      :style="{ backgroundColor: loadColor, borderRadius: bRadius }"
    ></view>

    <!-- 加载中 占位图 or 自定义加载loading 不用就注释 -->
    <view class="mys_slot" v-if="!resultImg && !imgError" :style="{ borderRadius: bRadius }">
      <image v-if="loadingPath" class="loading_img" :src="loadingPath"></image>
      <slot name="loading">
        <view v-if="isCustomLoading == 'c1'" class="custom_loading">
          <loading></loading>
          <view class="title" v-if="iscustomTitle">加载中</view>
        </view>
        <view v-if="isCustomLoading == 'c2'" class="custom_loading">
          <image class="custom_img" src="/static/images/loading.gif"></image>
          <view class="title" v-if="iscustomTitle">加载中</view>
        </view>
      </slot>
    </view>
    <!-- 自定义 加载失败的图片显示 -->
    <view class="mys_slot" v-if="imgError" :style="{ borderRadius: bRadius }">
      <image v-if="failPath" class="fail_img" :src="failPath"></image>
      <slot name="error"></slot>
    </view>
  </view>
</template>

<script>
function getID() {
  return "xxxxxxxx-xxxx-yxxx-yxxx-xxxxxxxxxxxx".replace(/[xy]/g, function (k) {
    let r = (Math.random() * 16) % 16 | 0;
    return (k == "x" ? r : (r & 0x3) | 0x8).toString(16);
  });
}
import loading from "@/components/mys_lazyImg/custom_loading/loading";

export default {
  components: { loading },
  props: {
    /* 图片 */
    imgUrl: String,
    imgId: String,
    imgMode: String, // 裁剪方式
    /* 盒子背景   */
    boxColor: {
      type: String,
      default: "#",
    },
    // loadingPath 配合 boxColor | loadMode 配合 loadColor | isCustomLoading 配合 boxColor ｜ 只有背景 boxColor 无加载性能最好 四选一 ！！！
    /* loadMode loading背景  （loadColor 背景色和 boxColor 一样时影响效果  使用只传 loadColor） */
    loadColor: {
      type: String,
      default: "#f5f5f5",
    },
    /* 加载中 图片站位图   */
    loadingPath: {
      type: [String, Boolean],
      default: false,
    },
    /* 加载中  css效果 可选 loading1 - 可扩展   */
    loadMode: {
      type: [String, Boolean],
      default: false,
    },
    /* 加载中 自定义loading - c1 c2 可扩展  */
    isCustomLoading: {
      type: [String, Boolean],
      default: false,
    },

    /* 加载完成 */
    /* 动画延时  注意： 为 ` 0s `  及无效果 */
    showDuration: {
      type: String,
      default: "0.5s",
    },
    /* 加载失败 */
    /* 定义图片路径  */
    failPath: {
      type: [String, Boolean],
      default: false,
    },

    /* 自定义宽高 ，css默认100% */
    imgW: {
      type: String,
      default: "",
    },
    imgH: {
      type: String,
      default: "",
    },
    /* 圆角 */
    bRadius: {
      type: String,
      default: "5px",
    },
    /* 开启监听scrollTop懒加载 */
    isLazy: {
      type: Boolean,
      default: true,
    },
    /*  距离顶部提前多少加载图片 例  已定位的coustom头部导航的高度   */
    coustomTop: {
      type: Number,
      default: 0,
    },
    scrollTop: Number,
    iscustomTitle: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      showImg: false,
      imgError: false,
      boxId: "",
      // 出现加载
      isLoad: false,
      // 加载成功
      resultImg: false,
    };
  },
  watch: {
    top(val) {
      if (val !== 0) {
        this.mScrollTop();
      }
    },
  },
  computed: {
    windowHeight() {
      return uni.getSystemInfoSync().windowHeight;
    },
    top() {
      if (this.isLoad) {
        return 0;
      } else {
        return this.scrollTop;
      }
    },
  },
  methods: {
    mLoad(e) {
      this.imgError = false;
      this.showImg = true;
     let timer = setTimeout(() => {
        this.resultImg = true;
        console.log("success");
        clearTimeout(timer);
        timer = null;
        // console.log("success", e.currentTarget.dataset.id);
      }, 50);
    },
    imageError(e) {
      this.imgError = true;
      console.log("err");
      // console.log("err", e.currentTarget.dataset.id);
    },
    mScrollTop(val) {
      if (this.isLoad == true) {
        return;
      }
      let id = this.boxId;
      let query = uni.createSelectorQuery().in(this);
      query
        .select("#" + id)
        .boundingClientRect((rect) => {
          if (!rect) return;
          if (rect.top - this.windowHeight < this.coustomTop) {
            this.isLoad = true;
          }
        })
        .exec();
    },
  },
  created() {
    this.boxId = "IMGID-" + getID();
  },
  mounted() {
    if (!this.isLazy) {
      this.isLoad = true;
    }
    // 首次加载
    this.$nextTick(this.mScrollTop);
  },
};
</script>

<style lang="scss" scoped>
/* 加载中  站位图片大小 */
.loading_img {
  width: 120rpx;
  height: 120rpx;
}
/* 加载失败自定义 图片大小 */
.fail_img {
  width: 120rpx;
  height: 120rpx;
}

/* 外部盒子 */
.mysImg_box {
  position: relative;
  // width: 100%;
  height: 100%;
  // background-color: transparent;
}
.mys_Img {
  width: 100%;
  height: 100%;
  opacity: 0;
}
.mTransition {
  transition: opacity 0.5s ease-in;
  opacity: 1;
}
.mTransitionNone {
  opacity: 1;
}

/* 自定义盒子 */
.mys_slot {
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
}

/* isCustomLoading 效果2 */
.custom_loading {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.title {
  font-size: 12px;
  color: #efefef;
}
.custom_img {
  width: 100rpx;
  height: 100rpx;
}

/* loading盒子 */
.loading {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
}

/* 若隐若现效果 */
.loading1 {
  background-color: #f4f5f5;
  border-radius: 8rpx;
  animation: load1 1s infinite ease-in-out;
  @keyframes load1 {
    0% {
      opacity: 1;
    }
    50% {
      opacity: 0.5;
    }
    100% {
      opacity: 1;
    }
  }
}
/* ...其他效果 */
/* 玻璃  */
// .loading2 {
//   background-color: #f3f3f3;
//   background-image: linear-gradient(-90deg, #efefef 0%, #fcfcfc 50%, #efefef 100%);
//   background-size: 400% 400%;
//   animation: load2 1s infinite;
//   @keyframes load2 {
//     0% {
//       background-position: 0% 0%;
//     }
//     100% {
//       background-position: -130% 0%;
//     }
//   }
// }
</style>
