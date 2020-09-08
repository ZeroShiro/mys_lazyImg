# mys_lazyImg

uniapp 图片懒加载

# 1.0.0

## 图片懒加载

### 使用方法

#### 引入

```javascript
import mysImg from "@/components/mys_lazyImg/mys_lazyImg";
Vue.component("mysImg", mysImg);
```

```html
<!-- 无加载效果 -->
<!-- 监听 scrollTop-->
<mysImg :imgUrl="imgUrl" :scrollTop="scrollTop" boxColor="#303030" />
<!-- 不监听直接显示 -->
<mysImg :imgUrl="imgUrl" :isLazy="false" boxColor="#303030" />

<!-- 加载中效果1 -->
<mysImg :imgUrl="imgUrl" :scrollTop="scrollTop" loadMode="loading1" loadColor="red" />
<!-- 加载中站位图 -->
<mysImg :imgUrl="imgUrl" :scrollTop="scrollTop" loadingPath="/static/images/xxx.jpg" boxColor="red" />
<!-- 自定义loading isCustomLoading c1 c2 or slot-->
<mysImg :imgUrl="imgUrl" :scrollTop="scrollTop" isCustomLoading="c1" boxColor="red" />

<mysImg :imgUrl="imgUrl" :scrollTop="scrollTop" boxColor="#fff">
  <view slot="loading" class="text-red">loading</view>
</mysImg>
```

! isCustomLoading c2
! failPath
需要 自行定义图片
站位图 失败图 大小 css 定义

#### props 值说明

|      属性       | 默认值 |   可选   |       类型        |                  简介                   |
| :-------------: | :----- | :------: | :---------------: | :-------------------------------------: |
|     imgUrl      | -      |    -     |      String       |                图片地址                 |
|      imgId      | -      |    -     |      String       |                 图片 id                 |
|     imgMode     | -      |    -     |      String       |              图片裁剪方式               |
|    boxColor     | -      |    -     |      String       |               盒子背景色                |
|    loadColor    | -      |    -     |      String       |             loadMode 背景色             |
|    loadMode     | false  | loading1 | [String, Boolean] |             加载中 css 效果             |
|   loadingPath   | false  |    -     | [String, Boolean] |         加载中 图片站位图 效果          |
| isCustomLoading | false  |  c1,c2   | [String, Boolean] |          加载中 自定义 loading          |
|  showDuration   | 0.5s   |    -     |      String       |  加载完成 图片动画显示延时（0s 为空 ）  |
|    failPath     | false  |    -     | [String, Boolean] |            加载失败图片路径             |
|      imgW       | -      |    -     |      String       |                图片宽高                 |
|      imgH       | -      |    -     |      String       |                图片宽高                 |
|     bRadius     | -      |    -     |      String       |                图片圆角                 |
|     isLazy      | -      |    -     |      Boolean      | 监听 scrolltop 开启（必须传 scrolltop） |
|   coustomTop    | 0      |    -     |      Number       |        距离顶部提前多少加载图片         |
|    scrollTop    | -      |    -     |      Number       |             滑动 scrollTop              |
|  iscustomTitle  | -      |    -     |      String       |               显示加载中                |
