<template>
  <div class="news">
    <banner title="新闻资讯" />
    <div class="news-section" v-loading="loading">
      <div class="news-section-content">
        <div class="content-nav">
          <div
            class="content-nav-btn"
            :class="{'content-nav-active':newstype == 1}"
            @click="newstype = 1"
          >
            <span>公司新闻</span>
          </div>
          <div
            class="content-nav-btn"
            :class="{'content-nav-active':newstype == 2}"
            @click="newstype = 2 "
          >
            <span>行业动态</span>
          </div>
        </div>

        <div class="content-nav-item">
          <router-link class="item-list" v-for="(item,index) in newsList" :key="index" :to="{ name: 'newsdetails', params: { item: item }}" >
            <div class="item-img" style="background-position-y: 1px;" v-lazy:background-image=" item.filePath"></div>

            <span class="item-list-title">{{item.title}}</span>
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Banner from "../components/Banner";
export default {
  name: "news",
  components: {
    Banner
  },
  data() {
    return {
      loading: true,
      newsList: [],
      newstype: 1
    };
  },
  methods: {
    loadData() {
      this.loading = true;
      this.$http
       .get(`news/GetNewsAll`)
       .then(response => {
          this.newsList = response.data.data.records;
          for(let a of this.newsList){
            a.filePath=this.imgserver+a.filePath
          }

          this.loading = false;
        })
       .catch(function(error) {
          window.console.log(error);
        });
    }
  },
  mounted() {
    this.loadData();
  },
  watch: {
    newstype(type) {
      window.console.log(type);
      this.loadData();
    }
  }
};
</script>

<style lang="scss" scoped>
* {
  margin: 0;
  padding: 0;
}
.news {
  width: 100%;
  height: 100%;
  background-color: #14679f;
  position: relative;
  overflow: hidden;

  &-section {
    width: 100%;

    &-content {
      width: 90vw;
      margin: 0 auto;
      background-color: #fff;
      border: 1px solid red;

      // 新增导航栏整体淡入动画
      animation: fadeInSectionContent 0.8s ease both;

    .content-nav {
        width: 80vw;
        height: 8vh;
        margin: 0 auto;
        display: flex;
        align-items: center;
        position: relative;
        bottom: 3vh;
        border: 1px solid red;

        &-btn {
          width: 50%;
          height: 100%;
          display: flex;
          align-items: center;
          justify-content: center;
          background-color: #e4e4e4;
          transition: all 0.2s;
          // 优化导航按钮淡入动画，增加延迟和弹性效果
          animation: fadeInNavBtn 0.6s ease both;
          animation-delay: 0.2s;

          &:hover {
            // 新增导航按钮悬停时的缩放动画
            animation: scaleOnHover 0.3s ease both;
          }
        }

      .content-nav-active {
          background-color: red;
          color: #fff;
          // 优化选中导航按钮的脉冲动画，增加弹性效果
          animation: pulseActive 0.6s ease both;
        }
      }

    .content-nav-item {
        width: 100%;
        margin: 0 auto;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;

      .item-list {
          width: 35vw;
          height: 45vh;
          display: flex;
          justify-content: center;
          align-items: center;
          background-color: #efefef;
          margin: 2vh 1vw;
          transition: all 0.3s ease;
          // 优化新闻列表项淡入向上动画，增加弹性效果
          animation: fadeInUpItem 0.8s ease both;
          animation-delay: 0.3s;

          &:hover {
            box-shadow: 0 0 5px 2px #bfd3e0;
            transform: scale(1.05);
            // 新增新闻列表项悬停时的背景色渐变动画
            animation: bgColorGradientOnHover 0.5s ease both;
          }

        .item-img {
            width: 100%;
            height: 100%;
            background-color: #cecece;
            background-repeat: no-repeat;
            background-size: cover;
            background-position: center;
            background-origin: content-box;
          }

        .item-list-title {
            width: 100%;
            height: 12vh;
            color: #15669e;
            font-size: 2.5vw;
            padding: 0 2vw;
            margin: 2vh 0;
            overflow: hidden;
            text-overflow: ellipsis;
            // 新增标题文字在加载时的淡入动画
            animation: fadeInTitle 0.6s ease both;
            animation-delay: 0.4s;
          }

        .item-content {
            width: 100%;
            height: 30vh;
            font-size: 1.5vw;
            color: gray;
            overflow: hidden;
            text-overflow: ellipsis;
            display: -webkit-box;
            -webkit-line-clamp: 5;
            -webkit-box-orient: vertical;
            white-space: normal!important;
            word-wrap: break-word;
          }

        .item-more {
            width: 100%;
            padding-top: 2vh;
            display: flex;
            justify-content: flex-start;
            align-items: center;

            img {
              width: 2vw;
              height: 2vw;
            }
            span {
              color: #e13834;
              padding: 0 1vw;
            }
          }
        }
      }
    }
  }

  // 新增动画定义
  @keyframes fadeInSectionContent {
    from {
      opacity: 0;
      transform: translateY(20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  @keyframes fadeInNavBtn {
    from {
      opacity: 0;
      transform: translateY(10px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  @keyframes scaleOnHover {
    from {
      transform: scale(1);
    }
    to {
      transform: scale(1.08);
    }
  }

  @keyframes pulseActive {
    0% {
      transform: scale(1);
    }
    50% {
      transform: scale(1.15);
    }
    100% {
      transform: scale(1);
    }
  }

  @keyframes fadeInUpItem {
    from {
      opacity: 0;
      transform: translateY(30px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  @keyframes bgColorGradientOnHover {
    from {
      background-color: #efefef;
    }
    to {
      background-color: #cceeff;
    }
  }

  @keyframes fadeInTitle {
    from {
      opacity: 0;
      transform: translateY(10px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  // 媒体查询，针对不同屏幕尺寸进行样式调整
  @media screen and (max-width: 768px) {
  .news {
      &-section {
        &-content {
        .content-nav {
            width: 90vw;
            height: 10vh;
            bottom: 2vh;

            &-btn {
              font-size: 2.5vw;
              // 调整导航按钮在小屏幕下的淡入动画延迟
              animation: fadeInNavBtn 0.4s ease both;
              animation-delay: 0.1s;
            }
          }

        .content-nav-item {
          .item-list {
              width: 90vw;
              height: 50vh;
              margin: 2vh 0;

              &-title {
                height: 8vh;
                font-size: 5vw;
                // 调整标题文字在小屏幕下的淡入动画延迟
                animation: fadeInTitle 0.4s ease both;
                animation-delay: 0.2s;
              }

              &-content {
                height: 30vh;
                font-size: 2.5vw;
                -webkit-line-clamp: 1;
              }

              &-more {
                padding-top: 3vh;
                img {
                  width: 4vw;
                  height: 4vw;
                }
                span {
                  font-size: 3vw;
                }
              }
            }
          }
        }
      }
    }
  }

  @media screen and (min-width: 769px) and (max-width: 1024px) {
  .news {
      &-section {
        &-content {
        .content-nav {
            width: 85vw;
            height: 7vh;
            bottom: 2.5vh;

            &-btn {
              font-size: 2vw;
              // 调整导航按钮在中等屏幕下的淡入动画延迟
              animation: fadeInNavBtn 0.6s ease both;
              animation-delay: 0.15s;
            }
          }

        .content-nav-item {
          .item-list {
              width: 40vw;
              height: 50vh;
              margin: 2vh 1vw;

              &-title {
                height: 10vh;
                font-size: 3vw;
                // 调整标题文字在中等屏幕下的淡入动画延迟
                animation: fadeInTitle 0.6s ease both;
                animation-delay: 0.20s;
              }

              &-content {
                height: 30vh;
                font-size: 2vw;
                -webkit-line-clamp: 4;
              }

              &-more {
                padding-top: 2.5vh;
                img {
                  width: 3vw;
                  height: 3vw;
                }
                span {
                  font-size: 2.5vw;
                }
              }
            }
          }
        }
      }
    }
  }

  @media screen and (min-width: 1025px) {
  .news {
      &-section {
        &-content {
        .content-nav {
            width: 400px;
            height: 55px;
            margin: 0 auto;
            display: flex;
            align-items: center;
            position: relative;
            bottom: 30px;
            border: 1px solid red;

            &-btn {
              width: 50%;
              height: 100%;
              display: flex;
              align-items: center;
              justify-content: center;
              background-color: #e4e4e4;
              transition: all 0.2s;
              // 调整导航按钮在大屏幕下的淡入动画延迟
              animation: fadeInNavBtn 0.4s ease both;
              animation-delay: 0.3s;
            }

          .content-nav-active {
              background-color: red;
              color: #fff;
              // 调整选中导航按钮的脉冲动画在大屏幕下的弹性效果
              animation: pulseActive 0.3s ease both;
              animation-delay: 0.1s;
            }
          }

        .content-nav-item {
            padding-bottom: 6vw;
          .item-list {
              text-decoration: none;
              color: inherit;
              width: 400px;
              height: 350px;
              display: flex;
              flex-direction: column;
              justify-content: center;
              align-items: center;
              margin: 10px 15px;
              transition: all 0.3s ease;
              // 调整新闻列表项在大屏幕下的淡入向上动画延迟
              animation: fadeInUpItem 0.8s ease both;
              animation-delay: 0.4s;

              &:hover {
              .item-list-title {
                  color: white;
                }
                background-color: #3388ff!important;
                transform: scale(1.05);
                // 调整新闻列表项悬停时的背景色渐变动画在大屏幕下的效果
                animation: bgColorGradientOnHover 0.5s ease both;
                animation-delay: 0.4s;
              }

              &-title {
                text-decoration: none!important;
                width: 100%;
                height: 3vw;
                line-height: 3vw;
                font-family: '宋体', sans-serif;
                text-align: center;
                margin: 0;
                color: #15669e;
                font-size: 22px;
                overflow: hidden;
                text-overflow: ellipsis;
                padding-bottom: 3px;
                // 调整标题文字在大屏幕下的淡入动画延迟
                animation: fadeInTitle 0.2s ease both;
                animation-delay: 0.1s;
              }

              &-content {
                width: 273px;
                height: 100px;
                font-size: 14px;
                color: gray;
                overflow:  334;
                text-overflow: ellipsis;
                display: -webkit-box;
                -webkit-line-clamp: 3;
                -webkit-box-orient: vertical;
                white-space: normal!important;
                word-wrap: break-word;
              }
            }
          }
        }
      }
    }
  }
}
</style>