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
    <div class="item-content-box">
      <div
          class="item-list-content"
          v-show="isContentVisible"
        >介绍：{{ content }}</div>
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
    // 初始化isContentVisible数组，长度与newsList相同，初始值都为false
    this.newsList.forEach(() => {
      this.isContentVisible.push(false);
    });
   
  },
  methods: {
    loadData() {
      this.loading = true;
      this.$http
      .get(`business/list`)
      .then(response => {
          //console.log(response);
          this.newsList = response.data.data;
          for(let obj of this.newsList){
            obj.filePath=this.imgserver+obj.filePath
          }
          this.content = this.newsList[0].description
          this.loading = false;
          //window.console.log(this.newsList);
        })
      .catch(function (error) {
          window.console.log(error);
        });
    },  
    routeLink(row) {
      this.$router.push({ name: "Tabe", query: { id: row } });
    },
  showContent(index) {
    const item  = this.newsList[index];
    this.content = item.description
    this.isContentVisible = true;
  },
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
.product {
  width: 100%;
  height: 100%;
  background-color: #14679f;
}

.product-content {
  width: 80vw; // 改为相对单位，占屏幕宽度的90%
  margin: 0 auto;
  background-color: #fff;
}

.p-video {
  width: 50vw; // 改为相对单位，占屏幕宽度的80%
  margin: 0 auto;
  padding: 5vh 0; // 改为相对单位，上下内边距为屏幕高度的5%
}

.content-modular-item {
  width: 100%;
  margin: 0 auto;
  display: flex; // 修改为flex布局
  flex-direction: row; // 设置为水平排列
  justify-content: center;
  flex-wrap: wrap;

 .item-list {
    width: 20vw; // 改为相对单位，占屏幕宽度的30%
    height: 30vh; // 改为相对单位，占屏幕高度的45%
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin: 2vh 1vw; // 改为相对单位，外边距上下为屏幕高度的2%，左右为屏幕宽度的1%
    border: 1px solid #15669e;
    

    &:hover {
      border: 1px solid #fff;
      box-shadow: 0 0 5px 2px #bfd3e0;
    }
    

   .item-img {
      width: 100%;
      height: 80%; // 占item-list高度的60%，保持宽高比自适应
      background-color: #cecece;
      background-repeat: no-repeat;
      background-size: cover;
      background-position: center;
      background-origin: content-box;
    }

    &-title {
      width: 100%;
      height: 2.1vh; // 改为相对单位，占屏幕高度的12%
      color: #15669e;
      font-size: 0.8vw; // 改为相对单位，根据屏幕宽度调整字体大小
      padding: 0 2vw;
      margin: 2vh 0;
      overflow: hidden;
      text-overflow: ellipsis;
      border-left: 1px solid #15669e;
    }

    &-content {
      width: 100%;
      height: 30vh; // 改为相对单位，占屏幕高度的30%
      font-size: 1.5vw; // 改为相对单位，根据屏幕宽度调整字体大小
      color: gray;

      // 文本长度处理 begin
      overflow: hidden;
      text-overflow: ellipsis;
      display: -webkit-box;
      -webkit-line-clamp: 5;
      -webkit-box-orient: vertical;
      white-space: normal!important;
      word-wrap: break-word;
      // 文本长度处理 ending
    }

    &-more {
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
.item-content-box{
  width: 100%;
  margin: 0 auto;
  display: flex; // 修改为flex布局
  flex-direction: row; // 设置为水平排列
  justify-content: center;

  .item-list-content{
  
    width: 50%;
      height: 30vh; // 改为相对单位，占屏幕高度的30%
      font-size: 1vw; // 改为相对单位，根据屏幕宽度调整字体大小
      color: gray;

      // 文本长度处理 begin
      overflow: hidden;
      text-overflow: ellipsis;
      display: -webkit-box;
      -webkit-line-clamp: 5;
      -webkit-box-orient: vertical;
      white-space: normal!important;
      word-wrap: break-word;
}

}




//公共样式
.title,
.eTitle {
  font-size: 4vw; // 改为相对单位，根据屏幕宽度调整字体大小
  font-weight: 400;
  color: 333;
  text-align: center;
}

.eTitle {
  font-size: 3.5vw; // 改为相对单位，根据屏幕宽度调整字体大小
  padding: 1vh 0; // 改为相对单位，上下内边距为屏幕高度的1%
}

// 媒体查询，针对不同屏幕尺寸进行样式调整
@media screen and (max-width: 768px) {
 .product {
    &-content {
      width: 95vw; // 在小屏幕下进一步调整内容宽度占比，占屏幕宽度的95%
    }

   .p-video {
      width: 90vw; // 在小屏幕下进一步调整视频宽度占比，占屏幕宽度的90%
      padding: 4vh 0; // 在小屏幕下进一步调整上下内边距，占屏幕高度的4%
    }

   .lacking {
      height: 50vh; // 在小屏幕下进一步调整高度占比，占屏幕 height的50%
      padding: 3vh 0; // 在小屏幕下进一步调整上下内边距，占 screen height的3%

     .lacking-bottom {
        padding: 6vh 6vw; // 在小屏幕下进一步调整内边距，占 screen height的6%，左右内边距为 screen width的6%

       .lacking-bottom-item {
          width: 30vw; // 在小屏幕下进一步调整宽度占比，占 screen width的30%

         .lacking-bottom-item-img {
            width: 15vw; // 在小屏幕下进一步调整宽度占比，占 screen width的15%
            height: 15vw; // 在小screen下进一步调整高度占比，占 screen height的15%
            line-height: 20vw; // 在小screen下进一步调整行高，根据元素大小调整行高
          }
        }
        p {
          font-size: 3vw; // 在小screen下进一步调整字体大小，根据 screen width调整字体大小
          margin: 1.5vh 0; // 在小screen下进一步调整上下内边距，占 screen height的1.5%
        }
      }
    }

   .target {
      margin-top: 4vh; // 在小screen下进一步调整上边距，占 screen height的4%

      &-img {
        margin-top: 4vh; // 在小screen下进一步调整上边距，占 screen height的4%
      }
    }

   .modular {
      margin-top: 4vh; // 在小screen下进一步调整上边距，占 screen height的4%

     .modular-content {
        padding: 4vh 4vw; // 在小screen下进一步调整内边距，占 screen height的4%，左右内边距为 screen width的4%
      }
    }

   .modular-content-same {
      width: 35vw; // 在小screen下进一步调整宽度占比，占 screen width的35%
      height: 45vh; // 在小screen下进一步调整高度占比，占 screen height的45%

     .same-item {
        height: 12vh; // 在小screen下进一步调整 height占比，占 screen height的12%
        padding: 1.5vh 0; // 在小screen下进一步调整上下内边距，占 screen height的1.5%

       .same-item-img {
          width: 5vw; // 在小screen下进一步调整 width占比，占 screen width的12%
          height: 5vw; // 在小screen下进一步调整 height占比，占 screen height的12%
          line-height: 16vw; // 在小screen下进一步调整行高，根据 element size调整行高
        }
        p {
          font-size: 3.5vw; // 在小screen下进一步调整 font size，根据 screen width调整字体大小
          line-height: 12vh; // 在小screen下进一步调整行高，根据 element size调整行高
        }
      }
    }

   .modular-content-center {
      width: 50vw; // 在小screen下进一步调整 width占比，占 screen width的50%
      height: 45vh; // 在小screen下进一步调整 height占比，占 screen height的45%
      text-align: center;
    }

   .title,
   .eTitle {
      font-size: 5vw; // 在小screen下进一步调整 font size，根据 screen width调整字体大小
    }
   .eTitle {
      padding: 1.5vh 0; // 在小screen下进一步调整上下内边距，占 screen height的1.5%
    }
  }
}

@media screen and (min-width: 769px) and (max-width: 1024px) {
 .product {
    &-content {
      width: 85vw; // 在中等屏幕下调整 content width占比，占 screen width的85%
    }

   .p-video {
      width: 75vw; // 在中等屏幕下调整 video width占比，占 screen width的75%
      padding: 4.5vh 0; // 在中等屏幕下调整上下内边距，占 screen height的4.5%
    }

   .lacking {
      height: 45vh; // 在中等屏幕下调整 height占比，占 screen height的45%
      padding: 2.5vh 0; // 在中等屏幕下调整上下内边距，占 screen height的2.5%

     .lacking-bottom {
        padding: 7vh 7vw; // 在中等屏幕下调整内边距，占 screen height的7%，左右内边距为 screen width的7%

       .lacking-bottom-item {
          width: 25vw; // 在中等屏幕下调整 width占比，占 screen width的25%

         .lacking-bottom-item-img {
            width: 14vw; // 在中等屏幕下调整 width占比，占 screen width的14%
            height: 14vw; // 在中等屏幕下调整 height占比，占 screen height的14%
            line-height: 18vw; // 在中等屏幕下调整行高，根据 element size调整行高
          }
        }
        p {
          fontSize: 2.5vw; // 在中等屏幕下调整 font size，根据 screen width调整字体大小
          margin: 1.2vh 0; // 在中等屏幕下调整上下内边距，占 screen height的1.2%
        }
      }
    }

   .target {
      margin-top: 4.5vh; // 在中等屏幕下调整上边距，占 screen height的4.5%

      &-img {
        margin-top: 4.5vh; // 在中等 screen下调整上边距，占 screen height的4.5%
      }
    }

   .modular {
      margin-top: 4.5vh; // 在中等 screen下调整上边距，占 screen height的4.5%

     .modular-content {
        padding: 4.5vh 4.5vw; // 在中等 screen下调整内边距，占 screen height的4.5%，左右内边距为 screen width的4.5%
      }
    }
  }
}
</style>