<template>
  <div class="product">
    <banner img="../assets/img/bgtop.jpg" title="业务介绍" />
    <div class="product-content" v-loading="loading">
      <div class="p-video">
        <video-player
          class="video-player vjs-custom-skin"
          ref="videoPlayer"
          :playsinline="true"
          :options="playerOptions"
        ></video-player>
      </div>
      <!--业务模块  -->
      <div class="modular">
        <h3 class="title">业务模块</h3>
        <p class="eTitle">MODULAR</p>
        <div class="content-modular-item">
          <div
            class="item-list"
            v-for="(item, index) in newsList"
            :key="index"
            @mouseenter="showContent(index)"
          >
            <div class="item-img" @click="routeLink(item.id)" v-lazy:background-image="item.filePath"></div>
            <p class="item-list-title">{{ item.name }}</p>
          </div>
        </div>
       
    </div>


    <div class="slip">
      <span class="slip-detail">业务详情</span>
    </div>
    
    <div class="item-content-box" >
      <div
          class="item-list-content"
          v-show="isContentVisible"
          :key="imgSrc"
        >
        <div class="item-list-img">
          <img :src="imgSrc" alt="" style="width: 100%;">
        </div>
        <div class="item-list-text">
          <span style="font-weight: 600;">业务介绍：</span>{{ content }}
        </div>
        </div>
      </div>
    </div>
   
  </div>
</template>

<script>
import Banner from "../components/Banner";
export default {
  data() {
    return {
      loading: true,
      videoSrc: "http://www.shkjem.com/video/kj.mp4",
      newsList: [],
      content:[],
      imgSrc:"",
      playerOptions: {
        playbackRates: [0.5, 1.0, 1.5, 2.0, 4.0, 8.0], //可选择的播放速度
        autoplay: false, //如果true,浏览器准备好时开始回放。
        muted: false, // 默认情况下将会消除任何音频。
        loop: false, // 视频一结束就重新开始。
        preload: "auto", // 建议浏览器在<video>加载元素后是否应该开始下载视频数据。auto浏览器选择最佳行为,立即开始加载视频（如果浏览器支持）
        language: "zh-CN",
        aspectRatio: "16:9", // 将播放器置于流畅模式，并在计算播放器的动态大小时使用该值。值应该代表一个比例 - 用冒号分隔的两个数字（例如"16:9"或"4:3"）
        fluid: true, // 当true时，Video.js player将拥有流体大小。换句话说，它将按比例缩放以适应其容器。
        sources: [
          {
            type: "",
            src: "http://www.shkjem.com/video/kj.mp4" //url地址
          }
        ],
        //poster: "../assets/img/home_top.jpg", //你的封面地址
        // width: document.documentElement.clientWidth,
        notSupportedMessage: "此视频暂无法播放，请稍后再试", //允许覆盖Video.js无法播放媒体源时显示的默认信息。
        controlBar: {
          timeDivider: true, //当前时间和持续时间的分隔符
          durationDisplay: true, //显示持续时间
          remainingTimeDisplay: false, //是否显示剩余时间功能
          fullscreenToggle: true //全屏按钮
        }
      },
      isContentVisible: [] // 用于记录每个item的内容是否可见
    };
  },
  components: {
    Banner
  },
  mounted() {
    this.loadData();
  },
  methods: {
    loadData() {
      this.loading = true;
      this.$http
        .get(`business/list`)
        .then(response => {
          this.newsList = response.data.data;
          for (let obj of this.newsList) {
            obj.filePath = this.imgserver + obj.filePath;
          }
          // 设置第一个内容的图片和描述
          this.loading = false;

          // 初始化isContentVisible数组，长度与newsList相同，初始值都为false
          this.isContentVisible = Array(this.newsList.length).fill(false);
          // 设置第一个内容为可见
          setTimeout(() => {
            this.imgSrc = this.newsList[0].filePath;
            this.content = this.newsList[0].description;
            this.isContentVisible[0] = true;
            this.$nextTick(() => {
              document.querySelector('.item-list-content').classList.add('active');
            });
          }, 500);

          
        })
        .catch(function (error) {
          window.console.log(error);
        });
    },  
    routeLink(row) {
      this.$router.push({ name: "Tabe", query: { id: row } });
    },
    showContent(index) {
      const item = this.newsList[index];
      this.content = item.description;
      console.log(item.filePath);
      this.imgSrc = item.filePath;
      this.isContentVisible = true;
      // 添加以下代码，为显示内容的盒子添加active类名以触发动画
      this.$nextTick(() => {
        document.querySelector('.item-list-content').classList.add('active');
      });
    }
  },
  created() {
    this.$http
    .get(
        "DataDictionary/GetDataDictionaryAll?key=%E4%BA%A7%E5%93%81%E8%A7%86%E9%A2%91%E9%93%BE%E6%8E%A5"
      )
    .get(
        "DataDictionary/GetDataDictionaryAll?key=%E4%BA%A7%E5%93%81%E8%A7%86%E9%A2%91%E9%93%BE%E6%8E%A5"
      )
    .then(response => {
        this.videoSrc = response.data[0].Content;
        window.console.log(this.videoSrc);
        this.loading = false;
      })
    .catch(function (error) {
        window.console.log(error);
      });
  }
};
</script>

<style lang="scss" scoped>
// 动画定义
@keyframes slideIn {
  from {
    transform: translateX(-8vw);
    opacity: 0;
  }
  to {
    transform: translateX(8vw);
    opacity: 1;
  }
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fadeInLeft {
  from {
    opacity: 0;
    transform: translateX(-20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.slip {
  width: 80%;
  height: 3vw;
  background-color: #f5f5f5;
  transform: translateX(8vw);
  animation: slideIn 0.5s ease;

.slip-detail {
    font-size: 1.5vw;
    color: #3388ff;
    width: 10vw;
    border-top: 4px solid #3388ff;
    background-color: white;
    text-align: center;
    line-height: 3vw;
    height: inherit;
    display: block;
  }
}

.product {
  width: 100%;
  height: 100%;
  background-color: #14679f;
}

.product-content {
  width: 80vw;
  margin: 0 auto;
  background-color: #fff;
}

.p-video {
  width: 50vw;
  margin: 0 auto;
  padding: 5vh 0;
}

.content-modular-item {
  width: 100%;
  margin: 0 auto;
  display: flex;
  flex-direction: row;
  justify-content: center;
  flex-wrap: wrap;

.item-list {
    width: 20vw;
    height: 30vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    background-color: #efefef;
    align-items: center;
    margin: 2vh 1vw;
    transition: all 0.3s ease;
    animation: fadeInUp 0.5s ease;

    &:hover {
      background-color: lightblue;
   .item-list-title {
        color: white!important;
      }
      transform: translateY(-5px);
      box-shadow: 0 5px 10px rgba(0, 0, 0, 0.3);
    }

 .item-img {
      width: 100%;
      height: 86%;
      background-color: #cecece;
      background-repeat: no-repeat;
      background-size: cover;
      background-position: center;
      background-origin: content-box;
    }

 .item-list-title {
      width: 100%;
      height: 2.1vh;
      color: black;
      font-size: 1vw;
      padding: 0 3vw;
      padding-bottom: 0.5vh;
      margin: 1vh 0;
      text-overflow: ellipsis;
      text-align: center;
      transition: all 0.3s ease;
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
      opacity: 0;
      transition: all 0.3s ease;
      transform: translateY(10px);
    }

 .item-more {
      width: 100%;
      padding-top: 2vh;
      display: flex;
      justify-content: flex-start;
      align-items: center;
      opacity: 0;
      transition: all 0.3s ease;
      transform: translateY(10px);

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

  &:hover {
 .item-content {
      opacity: 1;
      transform: translateY(0);
    }
 .item-more {
      opacity: 1;
      transform: translateY(0);
    }
  }
}

.item-content-box {
  width: 100%;
  margin: 0 auto;
  display: flex;
  flex-direction: column; // 修改为列布局
  justify-content: center; // 垂直居中
  align-items: center; // 水平居中

.item-list-content {
    width: 80%; // 适当调整宽度
    transform: translateX(0); // 移除不必要的偏移
    display: flex;
    justify-content: space-between;
    height: 30vh;
    font-size: 1vw;
    color: gray;
    height: 50vw;
    transition: all 0.5s ease;
    animation: fadeInLeft 0.5s ease;
    opacity: 0;
    transform: translateX(-20px);

    &.active {
      opacity: 1;
      transform: translateX(0);
    }

 .item-list-img {
      width: 50%;
      height: 30vw;
      display: block;
    }

 .item-list-img img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

 .item-list-text {
      width: 50%;
      font-family: '宋体', sans-serif;
      padding: 1vw;
      line-height: 1.5;
      color: #333;
      letter-spacing: 0.05em;
    }
  }
}

//公共样式
.title,
.eTitle {
  font-size: 4vw;
  font-weight: 400;
  color: 333;
  text-align: center;
}

.eTitle {
  font-size: 3.5vw;
  padding: 1vh 0;
}

// 媒体查询，针对不同屏幕尺寸进行样式调整
@media screen and (max-width: 768px) {
.product {
    &-content {
      width: 95vw;
    }

 .p-video {
      width: 90vw;
      padding: 4vh 0;
    }

 .lacking {
      height: 50vh;
      padding: 3vh 0;

   .lacking-bottom {
        padding: 6vh 6vw;

     .lacking-bottom-item {
          width: 30vw;

       .lacking-bottom-item-img {
            width: 15vw;
            height: 15vw;
            line-height: 20vw;
          }
        }
        p {
          font-size: 3vw;
          margin: 1.5vh 0;
        }
      }
    }

 .target {
      margin-top: 4vh;

      &-img {
        margin-top: 4vh;
      }
    }

 .modular {
      margin-top: 4vh;

   .modular-content {
        padding: 4vh 4vw;
      }
    }

 .modular-content-same {
      width: 35vw;
      height: 45vh;

   .same-item {
        height: 12vh;
        padding: 1.5vh 0;

     .same-item-img {
          width: 5vw;
          height: 5vw;
          line-height: 16vw;
        }
        p {
          font-size: 3.5vw;
          line-height: 12vh;
        }
      }
    }

 .modular-content-center {
      width: 50vw;
      height: 45vh;
      text-align: center;
    }

 .title,
 .eTitle {
      font-size: 5vw;
    }
 .eTitle {
      padding: 1.5vh 0;
    }
  }
}

@media screen and (min-width: 769px) and (max-width: 1024px) {
.product {
    &-content {
      width: 85vw;
    }

 .p-video {
      width: 75vw;
      padding: 4.5vh 0;
    }

 .lacking {
      height: 45vh;
      padding: 2.5vh 0;

   .lacking-bottom {
        padding: 7vh 7vw;

     .lacking-bottom-item {
          width: 25vw;

       .lacking-bottom-item-img {
            width: 14vw;
            height: 14vw;
            line-height: 18vw;
          }
        }
        p {
          fontSize: 2.5vw;
          margin: 1.2vh 0;
        }
      }
    }

 .target {
      margin-top: 4.5vh;

      &-img {
        margin-top: 4.5vh;
      }
    }

 .modular {
      margin-top: 4.5vh;

   .modular-content {
        padding: 4.5vh 4.5vw;
      }
    }
  }
}
</style>