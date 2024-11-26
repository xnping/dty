<template>
  <div class="NewsDetails">
    <banner img="../assets/img/bgtop.jpg" />
    <div class="NewsDetails-product">
      <div class="NewsDetails-product-content">
        <img :src="newsIdList.filePath" alt />
        <p class="product-title">{{ newsIdList.title }}</p>
        <p class="product-content">{{ newsIdList.content }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import Banner from "../components/Banner";
import axios from 'axios'; // 假设你使用axios进行网络请求，如果不是请替换为你实际使用的请求库

export default {
  name: "NewsDetails",
  components: {
    Banner
  },
  data() {
    return {
      item: {},
      newsIdList: {}
    };
  },
  created() {
    this.item = this.$route.params.item;
    console.log(this.item);
    // 这里可以添加根据获取到的item信息进一步请求详细数据的逻辑，比如：
    // this.fetchFullNewsDetails(this.item.id);
  },
  mounted() {
    this.loadData();
  },
  methods: {
    loadData() {
      // 这里假设你从路由参数中获取到的item已经包含了完整的新闻详情数据
      // 如果实际情况是需要再次请求获取详细数据，可以像下面这样写（以axios为例）：
      // axios.get(`your-api-endpoint-to-get-news-details/${this.item.id}`)
      // .then(response => {
      //     this.newsIdList = response.data;
      //   })
      // .catch(error => {
      //     console.error(error);
      //   });
      this.newsIdList = this.item;
    }
  }
};
</script>

<style lang="scss" scoped>
// 整体页面背景颜色设置为更柔和的深蓝色调，比之前更显高级
.NewsDetails {
  width: 100%;
  height: 100%;
  background-color: #0d253e;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  // 页面加载时的淡入动画
  animation: fadeInPage 0.8s ease both;

  &-product {
    width: 80vw;
    margin: 0 auto;
    background-color: #fff;
    box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    overflow: hidden;

    // 产品区域淡入动画，稍晚于页面整体淡入
    animation: fadeInProduct 0.6s ease both;
    animation-delay: 0.2s;

    &-content {
      width: 80%;
      margin: 0 auto;
      padding: 3vh 0;
      display: flex;
      flex-direction: column;
      align-items: center;

      img {
        width: 100%;
        height: auto;
        max-height: 40vh;
        object-fit: cover;
        margin-bottom: 2vh;

        // 图片加载时的淡入动画
        animation: fadeInImage 0.6s ease both;
        animation-delay: 0.3s;

        &:hover {
          transform: scale(1.03);
        }
      }

     .product-title {
        font-size: 2.5vw;
        font-weight: 600;
        color: #2c3e50;
        margin-bottom: 1vh;
        font-family: '宋体', sans-serif;
        // 标题文字淡入动画
        animation: fadeInTitle 0.6s ease both;
        animation-delay: 0.4s;
      }

     .product-content {
        font-size: 1.8vw;
        color: #555;
        line-height: 1.5;
        text-align: center;

        // 内容文字淡入动画
        animation: fadeInContent 0.6s ease both;
        animation-delay: 0.5s;
      }
    }
  }
}

// 动画定义
@keyframes fadeInPage {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fadeInProduct {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fadeInImage {
  from {
    opacity: 0;
    transform: translateY(20px);
    scale: 0.8;
  }
  to {
    opacity: 1;
    transform: translateY(0);
    scale: 1;
  }
}

@keyframes fadeInTitle {
  from {
    opacity: 0;
    transform: translateY(20px);
    }
  to {
    opacity: 1;
    transform: translateY(0);
    }
}

@keyframes fadeInContent {
  from {
    opacity: 0;
    transform: translateY(20px);
    }
  to {
    opacity: 1;
    transform: translateY(0);
    }
}

// 媒体查询，针对不同屏幕尺寸进行样式调整
@media screen and (max-width: 768px) {
 .NewsDetails {
    &-product {
      &-content {
        img {
          max-height: 30vh;

          // 调整小屏幕下图片淡入动画延迟
          animation: fadeInImage 0.6s ease both;
          animation-delay: 0.2s;
        }

       .product-title {
          font-size: 3.5vw;
          // 调整小屏幕下标题文字淡入动画延迟
          animation: fadeInTitle 0.6s ease both;
          animation-delay: 0.3s;
        }

       .product-content {
          font-size: 2.5vw;
          // 调整小屏幕下内容文字淡入动画延迟
          animation: fadeInContent 0.6s ease both;
          animation-delay: 0.4s;
        }
      }
    }
  }
}

@media screen and (min-width: 769px) and (max-width: 1024px) {
 .NewsDetails {
    &-product {
      &-content {
        img {
          max-height: 35vh;

          // 调整中等屏幕下图片淡入动画延迟
          animation: fadeInImage 0.6s ease both;
          animation-delay: 0.25s;
        }

       .product-title {
          font-size: 3vw;
          // 调整中等屏幕下标题文字淡入动画延迟
          animation: fadeInTitle 0.6s ease both;
          animation-delay: 0.35s;
        }

       .product-content {
          font-size: 2.2vw;
          // 调整中等屏幕下内容文字淡入动画延迟
          animation: fadeInContent 0.6s ease both;
          animation-delay: 0.45s;
        }
      }
    }
  }
}
</style>