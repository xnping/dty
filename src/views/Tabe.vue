<template>
  <div class="case">
    <banner img="../assets/img/bgtop.jpg" title="业务详情" />
    <div class="case-section" v-loading="loading">
      <div class="case-section-content">
        <div class="case-section-content-list" v-for="(cas,index) in caseList" :key="index">
          <img v-lazy="cas.fileUrls" class="case-image" />
          <div class="content-list-abstract" :class="{'abstract-active' : index%2!=1}">
            <p class="abstract-title">{{cas.title}}</p>
            <p class="abstract-content">{{cas.description}}</p>
            <router-link class="more" :to="{ name: 'casedetails', params: { cas: cas.id }}">
              <div class="button">查看详情</div>
            </router-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Banner from "../components/Banner";
export default {
  components: {
    Banner
  },
  data() {
    return {
      loading: true,
      id: 0,
      caseList: []
    };
  },
  created() {
    this.id = this.$route.query.id;
    window.console.log(this.id);
  },
  mounted() {
    this.$http
      .get(`case/getCases?id=${this.id}`)
      .then(response => {
        this.caseList = response.data.data;
        for (let obj of this.caseList) {
          obj.fileUrls = this.imgserver + obj.fileUrls.split(",")[0];
        }
        this.loading = false;
      })
      .catch(function(error) {
        window.console.log(error);
      });
  }
};
</script>

 <style lang="scss" scoped>
.case {
  width: 100%;
  height: 100%;
  position: relative;
  overflow: hidden;
  background-color: #14679f;

  &-section {
    width: 100%;
    &-content {
      width: 70vw;
      min-height: 50vh;
      margin: 0 auto;
      background-color: #fff;

      &-list {
        width: 100%;
        height: 40vh;
        display: flex;
        justify-content: center;
        align-items: center;
        border-bottom: 2px solid #efefef;
        transition: transform 0.3s ease;

        .case-image {
          width: 30vw;
          height: 33.3vh;
          transition: transform 0.3s ease, opacity 0.3s ease;
          opacity: 0.8;

          &:hover {
            transform: scale(1.05); // 鼠标悬停时放大图片
            opacity: 1; // 鼠标悬停时完全不透明
          }
        }

        .content-list-abstract {
          width: 30vw;
          height: 35vh;
          padding: 2vh 2vw;
          display: flex;
          flex-direction: column;
          justify-content: space-around;
          transition: transform 0.3s ease;

          &:hover {
            transform: translateY(-5px); // 鼠标悬停时上移效果
          }

          .abstract-title {
            line-height: 3vw;
            font-size: 2vw;
            color: #14679f;
            transition: color 0.3s ease;

            &:hover {
              color: #0a4e7d; // 鼠标悬停时改变颜色
            }
          }

          .abstract-content {
            height: 20vh;
            color: #484848;
            font-size: 1vw;
            overflow: hidden;
            text-overflow: ellipsis;
            display: -webkit-box;
            -webkit-line-clamp: 7;
            font-family: '宋体', sans-serif;
            letter-spacing: 0.05em;
            -webkit-box-orient: vertical;
            white-space: normal !important;
            word-wrap: break-word;
          }

          .more {
            display: flex;
            justify-content: flex-start;
            margin-top: 1rem; // 添加一些顶部间距
            background-color: #f5f5f5;
            text-decoration: none;

            &:hover {
              cursor: pointer;
              .button{
                // box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2); // 阴影效果
                background: linear-gradient(100deg, #f5f5f5, #c7c5c5); // 悬停时的渐变背景
                transform: translateX(24vw); // 悬停时的上移效果
                color: white;
              }
            }

            .button {
              display: inline-block;
              padding: 0.75rem 1.5rem; // 内边距
              background: linear-gradient(135deg, white, #f5f5f5); // 渐变背景
              color: black; // 字体颜色
              text-decoration: none; // 去掉下划线
              font-size: 1rem; // 字体大小
              font-weight: bold; // 加粗字体
              font-family: 宋体;
              transition: background 0.8s ease, transform 0.8s ease; // 添加过渡效果

              &:active {
                transform: translateX(0); // 点击时恢复位置
              }
            }
          }
        }

        .abstract-active {
          order: -1;
        }
      }
    }
  }
}


// 媒体查询，针对不同屏幕尺寸进行样式调整
@media screen and (max-width: 768px) {
  .case {
    &-section {
      &-content {
        &-list {
          .case-image {
            width: 80vw; // 在小屏幕下调整图片宽度占比
            height: 25vh; // 在小屏幕下调整图片高度占比
          }

          .content-list-abstract {
            width: 80vw; // 在小屏幕下调整内容宽度
            height: auto; // 自适应高度

            .abstract-title {
              line-height: 5vw; // 在小屏幕下调整行高
              font-size: 5vw; // 在小屏幕下调整字体大小
            }

            .abstract-content {
              font-size: 3vw; // 在小屏幕下调整字体大小
            }

            .more {
              .button {
                font-size: 4vw; // 在小屏幕下调整按钮字体大小
                padding: 0.5rem 1rem; // 在小屏幕下调整内边距
              }
            }
          }
        }
      }
    }
  }
}
</style>