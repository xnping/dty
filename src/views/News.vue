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
          <div class="item-list" v-for="(item,index) in newsList" :key="index">
            <div class="item-img" v-lazy:background-image=" item.filePath"></div>

            <p class="item-list-title">{{item.title}}</p>
            <p class="item-list-content">{{item.content}}</p>
            <div class="item-list-more">
              <router-link
                :to="{ name: 'newsdetails', params: { item: item }}"
              >
                <img src="../assets/img/sanjiao.png" />
                <span>more</span>
              </router-link>
            </div>
          </div>
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
           window.console.log(this.newsList)
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
      width: 90vw; // 改为相对单位，占屏幕宽度的90%
      margin: 0 auto;
      background-color: #fff;
      border: 1px solid red;

     .content-nav {
        width: 80vw; // 改为相对单位，占屏幕宽度的80%
        height: 8vh; // 改为相对单位，占屏幕高度的8%
        margin: 0 auto;
        display: flex;
        align-items: center;
        position: relative;
        bottom: 3vh; // 改为相对单位，占屏幕高度的3%
        border: 1px solid red;

        &-btn {
          width: 50%;
          height: 100%;
          display: flex;
          align-items: center;
          justify-content: center;
          background-color: #e4e4e4;
          transition: all 0.2s;
        }

       .content-nav-active {
          background-color: red;
          color: #fff;
        }
      }

     .content-nav-item {
        width: 100%;
        margin: 0 auto;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;

       .item-list {
          width: 35vw; // 改为相对单位，占屏幕宽度的30%
          height: 45vh; // 改为相对单位，占屏幕高度的45%
          display: flex;
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
            height: 60%; // 占item-list高度的60%，保持宽高比自适应
            background-color: #cecece;
            background-repeat: no-repeat;
            background-size: cover;
            background-position: center;
            background-origin: content-box;
          }

          &-title {
            width: 100%;
            height: 12vh; // 改为相对单位，占屏幕高度的12%
            color: #15669e;
            font-size: 2.5vw; // 改为相对单位，根据屏幕宽度调整字体大小
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
    }
  }
 .text-decoration {
    text-decoration: none;
  }

  // 媒体查询，针对不同屏幕尺寸进行样式调整
  @media screen and (max-width: 768px) {
   .news {
      &-section {
        &-content {
         .content-nav {
            width: 90vw; // 在小屏幕下进一步调整导航宽度占比，占屏幕宽度的90%
            height: 10vh; // 在小屏幕下进一步调整导航高度占比，占屏幕高度的10%
            bottom: 2vh; // 在小屏幕下进一步调整导航相对位置，占屏幕高度的2%

            &-btn {
              font-size: 2.5vw; // 在小屏幕下调整按钮字体大小，根据屏幕宽度调整字体大小
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
              }

              &-content {
                height: 30vh; 
                font-size: 2.5vw; 
                -webkit-line-clamp: 1; 
              }

              &-more {
                padding-top: 3vh; // 在小屏幕下进一步调整更多信息区域的上边距，占屏幕高度的3%
                img {
                  width: 4vw; // 在小屏幕下进一步调整箭头图标大小，根据屏幕宽度调整大小
                  height: 4vw;
                }
                span {
                  font-size: 3vw; // 在小屏幕下进一步调整更多信息文字大小，根据屏幕宽度调整字体大小
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
            }

           .content-nav-active {
              background-color: red;
              color: #fff;
            }
          }

         .content-nav-item {
           .item-list {
              width: 330px;
              height: 500px;
              display: flex;
              flex-direction: column;
              justify-content: center;
              align-items: center;
              margin: 10px 10px;
              border: 1px solid #15669e;

              &-title {
                width: 300px;
                height: 60px;
                color: #15669e;
                font-size: 22px;
                padding: 0 10px;
                margin: 20px 0;
                overflow: hidden;
                text-overflow: ellipsis;
                border-left: 1px solid #15669e;
              }

              &-content {
                width: 273px;
                height: 100px;
                font-size: 14px;
                color: gray;

                // 文本长度处理 begin
                overflow: hidden;
                text-overflow: ellipsis;
                display: -webkit-box;
                -webkit-line-clamp: 3;
                -webkit-box-orient: vertical;
                white-space: normal!important;
                word-wrap: break-word;
                // 文本长度处理 ending
              }

              &-more {
                width: 273px;
                padding-top: 20px;
                display: flex;
                justify-content: flex-start;
                align-items: center;

                img {
                  width: 12px;
                  height: 12px;
                }
                span {
                  color: #e13834;
                  padding: 0 5px;
                }
              }
            }
          }
        }
      }
    }
  }
}

</style>