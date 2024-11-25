<template>
  <div class="home" id="home" v-loading="loading">
    <swiper id="swiperBox" v-bind:options="swiperOption" ref="mySwiper">
      <swiper-slide class="swiper-slide slide-one">
        <div class="page">
          <h3>深圳市达通源科技</h3>
          <p>DATONE Technical</p>
        </div>
        <p class="slogan">公司文化 企业文化</p>
        <div class="mouse-scroll-hint">
          <div class="mouse-icon"></div>
          <div class="mouse-text">鼠标滚动</div>
        </div>
      </swiper-slide>
      <swiper-slide class="swiper-slide slide-two">
        <div class="page">
          <h3>经营业务</h3>
          <p>Line of Business</p>
        </div>
        <ul class="case-item">
          <li
            v-for="(cas, index) in caseList"
            :key="index"
            v-lazy:background-image="cas.filePath"
          >
            <router-link
              class="text-decoration"
              :to="{ name: 'casedetails', params: { cas: cas } }"
            >
              <div class="case-item-hover">
                <p class="hover-title">{{ cas.name }}</p>
                <div class="bottom"></div>
                <div class="more">
                  <span>MORE</span>
                </div>
              </div>
            </router-link>
          </li>
        </ul>
      </swiper-slide>
      <swiper-slide class="swiper-slide slide-three">
        <div class="page">
          <h3>最新资讯</h3>
          <p>Latest News</p>
        </div>
        <div class="news-content">
          <div class="news-content-item" v-for="(item, i) in newsList" :key="i">
            <div :style="'order: ' + (i % 2 == 0 ? 1 : 3)">
              <router-link
                class="text-decoration"
                :to="{ name: 'newsdetails', params: { item: item } }"
              >
                <div
                  class="item-img"
                  v-lazy:background-image="item.filePath"
                ></div>
              </router-link>
            </div>
            <div style="order: 2">
              <el-divider>
                <i class="el-icon-apple"></i>
              </el-divider>
            </div>
            <div class="item-content" :style="'order: ' + (i % 2 == 0 ? 3 : 1)">
              <h3>{{ item.title }}</h3>
              <p>{{ item.content }}</p>
              <span>{{ formatCreateTime(item.createTime) }}</span>
            </div>
          </div>
        </div>
      </swiper-slide>
    </swiper>
  </div>
</template>
 
<script>
import { swiper, swiperSlide } from "vue-awesome-swiper";
export default {
  name: "HelloWorld",
  components: {
    swiper,
    swiperSlide,
  },
  data() {
    return {
      loading: true,
      caseList: [],
      caseLiat:[],
      newsList: [],
      swiperOption: {
        notNextTick: true, //notNextTick是一个组件自有属性，如果notNextTick设置为true，组件则不会通过NextTick来实例化swiper，也就意味着你可以在第一时间获取到swiper对象，假如你需要刚加载遍使用获取swiper对象来做什么事，那么这个属性一定要是true
        direction: "vertical", //水平方向移动
        grabCursor: true, //鼠标覆盖Swiper时指针会变成手掌形状，拖动时指针会变成抓手形状
        setWrapperSize: true, //Swiper使用flexbox布局(display: flex)，开启这个设定会在Wrapper上添加等于slides相加的宽或高，在对flexbox布局的支持不是很好的浏览器中可能需要用到。
        autoHeight: true, //自动高度。设置为true时，wrapper和container会随着当前slide的高度而发生变化
        slidesPerView: 1, //设置slider容器能够同时显示的slides数量(carousel模式)。可以设置为数字（可为小数，小数不可loop），或者 'auto'则自动根据slides的宽度来设定数量。loop模式下如果设置为'auto'还需要设置另外一个参数loopedSlides。
        mousewheel: true, //开启鼠标滚轮控制Swiper切换。可设置鼠标选项，默认值false
        mousewheelControl: true, //同上
        height: window.innerHeight - 60, // 高度设置，占满设备高度
        resistanceRatio: 0, //抵抗率。边缘抵抗力的大小比例。值越小抵抗越大越难将slide拖离边缘，0时完全无法拖离。本业务需要
        observeParents: true, //将observe应用于Swiper的父元素。当Swiper的父元素变化时，例如window.resize，Swiper更新

        // 如果自行设计了插件，那么插件的一些配置相关参数，也应该出现在这个对象中，如下debugger
        debugger: true,

        // swiper的各种回调函数也可以出现在这个对象中，和swiper官方一样
      },
    };
  },
  methods: {
    formatCreateTime(timestamp) {
      const date = new Date(timestamp);
      return date.toLocaleString();
    }
},
  created() {
    this.$nextTick(() => {
      const sloganElement = document.querySelector(".slogan");
      if (sloganElement) {
        sloganElement.classList.add("show");
      }
    });
  },
  // 如果你需要得到当前的swiper对象来做一些事情，你可以像下面这样定义一个方法属性来获取当前的swiper对象，同时notNextTick必须为true
  computed: {
    swiper() {
      return this.$refs.mySwiper.swiper;
    },
  },
  mounted() {
   
    this.$http
      .all([
        this.$http.get(`business/list`),
        this.$http.get(`news/GetNewsAll`),
        this.$http.get(`/case/getCases?id=3`),
      ])
      .then(
        this.$http.spread((responseCases, responseNews, responserCasas) => {
          this.caseList = responseCases.data.data;
          for(let obj of this.caseList){
            obj.filePath=this.imgserver+obj.filePath
          }
          
         
          this.caseLiat = responserCasas.data.data;
          for(let obj of this.caseLiat){
            obj.fileUrls=this.imgserver+obj.fileUrls.split(",")[0]
          }
           this.newsList = responseNews.data.data.records;
           window.console.log(this.newsList)
          for(let a of this.newsList){
            a.filePath=this.imgserver+a.filePath
          }

          this.loading = false;
        })
      );
    
  }, 
};
</script>
 
<style lang="scss" scoped>
.home {
  width: 100%;
  height: 10%;
}
.swiper-slide {
  font-size: 24px;
  // text-align: center;
  // line-height: 100px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;

  .page {
    text-align: center;
    height: 100px;
    overflow: hidden;
    h3,
    p {
      font-size: 40px;
      font-weight: 400;
      color: #fff;
    }
  }

  .slogan {
    text-align: center;
    font-size: 50px;
    color: #fff;
    padding: 30px 0;
    transform: translateY(-50px); /* 初始位置在上方50px处，不可见 */
    opacity: 0;
    transition: transform 1s ease, opacity 1s ease; /* 设置位置和透明度的过渡效果，持续1秒，缓动方式为ease */
  }

  .slogan.show {
    transform: translateY(
      0
    ); /* 当添加.show类名时，位置回到正常，即垂直方向偏移量为0 */
    opacity: 1; /* 同时透明度变为1，完全可见 */
  }
  /* 整体提示容器 */
  .mouse-scroll-hint {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  /* 鼠标图标样式 */
  .mouse-icon {
    width: 20px;
    height: 40px;
    border: 2px solid rgb(255, 255, 255);
    border-radius: 100px;
    position: relative;
    animation: colorSlide-data-v-5aff6e40 6s linear infinite,
      nudgeMouse-data-v-5aff6e40 6s ease-out infinite;

    div{
    display: block;
    unicode-bidi: isolate;
}
.section[data-v-5aff6e40] {
    background-size: cover;
    background-position: 50%;
    color: #fff;
    font-weight: 400;
    height: 100%;
    text-align: center;
    padding-top: 60px;
}
  }

  .mouse-icon:before {
    content: "";
    width: 4px;
    height: 4px;
    border-radius: 50%;
    background-color: rgb(255, 254, 254);
    position: absolute;
    top: 10px;
    left: 40%;
    transform: translateX(-50%);
    animation: scroll-animate 6s infinite;
  }

  /* 动画效果 */
  @keyframes scroll-animate {
    0% {
      transform: translateY(0);
    }
    20% {
      transform: translateY(8px);
    }
    30% {
      transform: translateY(0);
    }
    50% {
      transform: translateY(8px);
    }
    60% {
      transform: translateY(0);
    }
    80% {
      transform: translateY(8px);
    }
    90% {
      transform: translateY(0);
    }
  }

  @keyframes colorSlide-data-v-5aff6e40 {
    0% {
      background-position: 0 100%;
    }
    20% {
      background-position: 0 0;
    }
    21% {
      background-color: #4e5559;
    }
    29.9% {
      background-color: #fff;
      background-position: 0 0;
    }
    30% {
      background-color: #4e5559;
      background-position: 0 100%;
    }
    50% {
      background-position: 0 0;
    }
    51% {
      background-color: #4e5559;
    }
    59.99% {
      background-color: #fff;
      background-position: 0 0;
    }
    60% {
      background-color: #4e5559;
      background-position: 0 100%;
    }
    80% {
      background-position: 0 0;
    }
    81% {
      background-color: #4e5559;
    }
    89.99%,
    100% {
      background-color: #fff;
    }
  }

  @keyframes nudgeMouse-data-v-5aff6e40 {
    0% {
      transform: translateY(0);
    }
    20% {
      transform: translateY(8px);
    }
    30% {
      transform: translateY(0);
    }
    50% {
      transform: translateY(8px);
    }

    80% {
      transform: translateY(8px);
    }
    90% {
      transform: translateY(0);
    }
  }

  @keyframes nudgeMouse-data-v-5aff6e40 {
    0% {
      transform: translateY(0);
    }
    20% {
      transform: translateY(8px);
    }
    30% {
      transform: translateY(0);
    }
    50% {
      transform: translateY(8px);
    }

    80% {
      transform: translateY(8px);
    }
    90% {
      transform: translateY(0);
    }
  }
  @keyframes colorText-data-v-5aff6e40 {
    21% {
    color: hsla(0, 0%, 100%, 0);
}
30% {
    color: #fff;
}
60% {
    color: #fff;
}
81% {
    color: hsla(0, 0%, 100%, 0);
}

90% {
    color: #fff;
}
  }

@keyframes  nudgeText-data-v-5aff6e40{
  
}

  /* 文本样式 */
  .mouse-text {
    padding-top: .889rem;
    font-size: .889rem;
    font-weight: 300;
    font-family: Microsoft YaHei, Lantinghei SC, Open Sans, Arial, Hiragino Sans GB, STHeiti, WenQuanYi Micro Hei, SimSun, sans-serif;
    letter-spacing: 12px;
    text-indent: 12px;
    color: #fff;
    animation: colorText-data-v-5aff6e40 5s ease-out infinite, nudgeText-data-v-5aff6e40 5s ease-out infinite;
  }
}
//经典案例
.case-item {
  width: 1100px;
  height: 500px;
  overflow: hidden;
  margin: 0 auto;
  margin-top: 30px;
  display: flex;
  flex-wrap: wrap;
  li {
    width: 350px;
    height: 210px;
    list-style: none;
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    background-origin: content-box;
    margin: 5px;
    position: relative;
    overflow: hidden;

    &:hover {
      .case-item-hover {
        opacity: 1;
        transition: all 0.4s ease-in-out;
      }
    }
  }
}
//经典案例hover
.case-item-hover {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  opacity: 0;
  overflow: hidden;
  background-color: rgba(225, 56, 52, 0.7);

  .hover-title {
    height: 50px;
    color: #fff;
    font-size: 18px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    font-weight: 400;
    margin-top: 20px;
  }
  .bottom {
    border-bottom: 1px solid #fff;
    width: 40px;
    margin: 0 auto;
  }
  .more {
    width: 90px;
    padding: 5px 5px;
    margin: 0 auto;
    margin-top: 80px;
    border: 2px solid #fff;
    span {
      color: #fff;
      font-size: 20px;
    }
  }
}

.slide-one {
  background: url(../assets/img/home_top.jpg) no-repeat center;
  background-size: cover;
}
.slide-two {
  background: url(../assets/img/home_do.jpg) no-repeat center;
  background-size: cover;
}
.slide-three {
  background: url(../assets/img/home_anli.jpg) no-repeat center;
  background-size: cover;
}
//最新资讯
.news-content {
  width: 1240px;
  margin: 0 auto;
  margin-top: 40px;
  display: flex;
  justify-content: center;

  &-item {
    width: 380px;
    display: flex;
    flex-direction: column;

    .item-img {
      width: 320px;
      height: 230px;
      background-repeat: no-repeat;
      background-size: cover;
      background-position: center;
      background-origin: content-box;
      margin: 0 auto;
    }
    .el-divider {
      background-color: #fff;
      height: 3px;
      .el-divider__text {
        width: 20px;
        height: 20px;
        border-radius: 50%;
        padding: 0px;
        color: #fff;
      }
    }
    .item-content {
      width: 360px;
      height: 230px;
      margin: 0 auto;
      //border: 1px solid paleturquoise;
      h3 {
        font-size: 20px;
        height: 30px;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
      }
      p {
        font-size: 14px;
        height: 80px;
        overflow: hidden;
        margin: 10px 0;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 3;
        -webkit-box-orient: vertical;
        white-space: normal !important;
        word-wrap: break-word;
      }
      span {
        display: block;
        font-size: 14px;
        text-align: end;
      }
      h3,
      p,
      span {
        color: #fff;
      }
    }
  }
}
.order {
  order: -1;
}
.order-img {
  order: 1;
}

// 不同屏幕尺寸下的样式调整，通过媒体查询实现
@media screen and (max-width: 768px) {
  .swiper-slide {
    font-size: 3vw;
    width: 200vw;
    height: 100vh;

    .page {
      height: 20vh;
      h3 {
        font-size: 5vw;
      }
      p {
        font-size: 3vw;
      }
    }
    .slogan {
      font-size: 6vw;
      padding: 3vw 0;
    }
  }

  .case-item {
    width: 95vw;
    height: 50vh;
    margin: 3vh auto;
    li {
      width: 100%;
      height: 50%;
      margin: 2vh 0;
    }
  }

  .case-item-hover {
    .hover-title {
      height: 5vw;
      font-size: 2vw;
      margin-top: 3vw;
    }
    .bottom {
      width: 5vw;
    }
    .more {
      width: 8vw;
      padding: 1.5vw 1.5vw;
      margin: 0 auto;
      margin-top: 15vw;
      border: 2px solid #fff;
      span {
        color: #fff;
        font-size: 2.5vw;
      }
    }
  }

  .news-content {
    width: 95vw;
    margin: 3vh auto;

    .news-content-item {
      width: 100%;

      .item-img {
        width: 100%;
        height: 50vh;
      }

      .el-divider {
        background-color: #fff;
        height: 1vw;
        .el-divider__text {
          width: 3vw;
          height: 3vw;
          borderradius: 50%;
          padding: 0px;
          color: #fff;
        }
      }

      .item-content {
        width: 100%;
        height: 50vh;
        h3 {
          font-size: 4vw;
          height: 6vw;
        }
        p {
          font-size: 3vw;
        }
        span {
          font-size: 2vw;
        }
      }
    }
  }
}
</style>