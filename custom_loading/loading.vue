<!-- 

Loading图
 
 -->
<template>
  <view class="loading_box_custom">
    <view></view>
    <view></view>
    <view></view>
    <view></view>
    <view></view>
    <view></view>
    <view></view>
    <view></view>
    <view></view>
    <view></view>
    <view></view>
    <view></view>
  </view>
</template>

<script>
export default {
  data() {
    return {};
  },
  methods: {},
  created() {},
};
</script>

<style lang="scss" scoped>
.loading_box_custom {
  color: official;
  display: inline-block;
  position: relative;
  width: 30px;
  height: 30px;
}

.loading_box_custom view {
  transform-origin: 16px 16px; //大小
  animation: loading_box_custom 1.2s linear infinite;
}

.loading_box_custom view:after {
  content: " ";
  display: block;
  position: absolute;
  top: 3px;
  left: 13px; // 比例 transform-origin - 3
  width: 4px;
  height: 6px;
  border-radius: 50%;
  background: #dbdbdb;
}

.loading_box_custom view:nth-child(1) {
  transform: rotate(0deg);
  animation-delay: -1.1s;
}

.loading_box_custom view:nth-child(2) {
  transform: rotate(30deg);
  animation-delay: -1s;
}

.loading_box_custom view:nth-child(3) {
  transform: rotate(60deg);
  animation-delay: -0.9s;
}

.loading_box_custom view:nth-child(4) {
  transform: rotate(90deg);
  animation-delay: -0.8s;
}

.loading_box_custom view:nth-child(5) {
  transform: rotate(120deg);
  animation-delay: -0.7s;
}

.loading_box_custom view:nth-child(6) {
  transform: rotate(150deg);
  animation-delay: -0.6s;
}

.loading_box_custom view:nth-child(7) {
  transform: rotate(180deg);
  animation-delay: -0.5s;
}

.loading_box_custom view:nth-child(8) {
  transform: rotate(210deg);
  animation-delay: -0.4s;
}

.loading_box_custom view:nth-child(9) {
  transform: rotate(240deg);
  animation-delay: -0.3s;
}

.loading_box_custom view:nth-child(10) {
  transform: rotate(270deg);
  animation-delay: -0.2s;
}

.loading_box_custom view:nth-child(11) {
  transform: rotate(300deg);
  animation-delay: -0.1s;
}

.loading_box_custom view:nth-child(12) {
  transform: rotate(330deg);
  animation-delay: 0s;
}

@keyframes loading_box_custom {
  0% {
    opacity: 1;
  }

  100% {
    opacity: 0;
  }
}
</style>
