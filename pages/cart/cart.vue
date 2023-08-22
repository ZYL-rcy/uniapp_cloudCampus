<template>
  <view class="wrap">
    <view class="menu">
      <view class="active">
        学校通知
      </view>
      <view class="active">
        师生快讯
      </view>
      <view class="active">
        社会新闻
      </view>
    </view>

    <view class="swiper-container">
      <uni-swiper :indicator-dots="true" :autoplay="false" :interval="500" @change="handleSwiperChange">
        <swiper-item v-for="(image, index) in imageList" :key="index">
          <image class="swiper-image" :src="image"></image>
        </swiper-item>
      </uni-swiper>
    </view>

    <view class="news-lists">
      <template v-for="(news, index) in newsList">
        <view class="news-item">
          <text class="news-title" @click="goToNewsPage(news)">{{ news }}</text>
        </view>
        <!-- 添加分割线 -->
        <view v-if="index !== newsList.length - 1" class="divider"></view>
      </template>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      title: "Hello",
      newsList: [
        "关于学校学生暑期返校通知",
        "如何看待哈圈乱象——最大的盘是否是路人盘？",
        "关于征集校歌歌词的通知",
        "关于统计各地学生家庭因暴雨受灾情况的通知",
        "关于组织参加2023年全国大学生数学建模大赛的通知",
        "桃李芬芳，不忘吐脯情深；携手共促，共筑复兴荣光",
        "关于开展首届一站式学生社区风采展示活动的通知",
        "保卫处开展假期值班巡查工作",
        "《人人讲安全，各个会应急》公益广告",
        "校内文明行车，规范停车倡议书",
        "图书馆2020-2023书目公示",
      ],
      imageList: [
        "/static/images/slide1.jpg",
        "/static/images/slide2.jpg",
        "/static/images/slide3.jpg",
      ],
    };
  },
  onLoad() {},
  methods: {
    goToNewsPage(news) {
      // 页面跳转
      uni.navigateTo({
        url: "/pages/news/news?id=" + news,
      });
    },
    handleSwiperChange(e) {
      // 获取当前轮播图的索引
      const current = e.detail.current;
      // 执行相关操作，例如更新当前选中的图片状态等
    },
  },
};
</script>

<style>
.wrap {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.menu {
  display: flex;
  width: 100%;
  border-bottom: 1px solid #bdd299;
}

.menu view {
  flex: 1;
  text-align: center;
  font-size: 15px;
  height: 40px;
  line-height: 40px;
  color: #808080;
  cursor: pointer;
}

.active {
  color: #bdd299;
  border-bottom: 2px #bdd299 solid;
}

.swiper-container {
  width: 100%;
  height: 200px; /* 修改为期望的高度 */
  margin-top: 10px;
}

.swiper-image {
  width: 100%;
  height: 200px;
}

.news-lists {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.news-item {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  justify-content: center;
  width: 90%;
  max-width: 600px;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 20px;
  margin-bottom: 10px;
  background-color: #fff;
}

.news-title {
  font-size: 20px;
  font-weight: bold;
  text-align: center;
  margin-bottom: 10px;
  color: #333;
  cursor: pointer;
}

.divider {
  width: 90%;
  height: 1px;
  background-color: #ddd;
  margin: 10px 0;
}
</style>