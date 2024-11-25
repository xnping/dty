<template>
  <div class="go-in">
    <banner img="../assets/img/bgtop.jpg" title="走进达通源" />
    <div class="section" v-loading="loading">
      <div class="section-content">
        <div class="content-summary">
          <div class="summary-left">
            <p class="title">公司简介</p>
            <p class="eTitle">ABOUT US</p>
            <p class="content">
              深圳市达通源科技有限公司，简称DTY，是国家认可的第三方检测机构。公司创立于2013年6月，依法注册取得工商营业执照，是一家具有独立法人的企业，主要致力于节水、节能等领域，为市场提供专业的检测、技术、咨询等服务。
              二、业务介绍经营范围主要包括水量平衡测试、节水型单位创建服务、节水改造及检测、节水宣传培训、合同节水；节能技术咨询、能效检测、节能评估、节能工程改造、节能宣传培训、节约型机关创建服务；能源审计、合同能源管理、能源计量审查技术服务；碳计量技术咨询等。
            </p>
          </div>
          <div class="summary-right">
            <img src="../assets/img/jianjietopmin.jpg" alt />
          </div>
        </div>
        <el-divider class="el-divider-active">
          <i class="el-icon-arrow-down el-icon-arrow-down-active"></i>
        </el-divider>

        <!-- 公司荣誉 -->
        <div class="content-honor">
          <div class="honor-big-img">
            <el-dialog :title="dialogTitle" :visible.sync="dialogTableVisible">
              <img v-lazy="dialogUrl" alt />
            </el-dialog>
          </div>
          <div class="top">
            <h3>公司荣誉</h3>
            <p>HONOR</p>
            <div class="border"></div>
          </div>
          <ul class="honor-show">
            <li v-for="(honor, index) in honorList" :key="index">
              <img
                v-lazy="honor.filePath"
                @click="
                  dialogTableVisible = true;
                  dialogUrl = honor.filePath;
                  dialogTitle = honor.Remark;
                "
              />
            </li>
          </ul>
          <p>点击图片查看大图</p>
        </div>

        <!-- 合作伙伴 -->
        <div class="content-partner">
          <div class="top">
            <h3>合作伙伴</h3>
            <p>RARTNERS</p>
            <ul class="partner-img">
              <!-- <li v-for="(partner,i) in partnerImg" :key="i">
                <img :src="partner.Img" alt />
              </li> -->
              <li>
                <img src="../assets/img/img1.jpg" alt />
                <p style="color: #14679f; font-size: 14px">
                  广东内伶仃福田国家级自然保护区管理局
                </p>
              </li>
              <li>
                <img src="../assets/img/img2.jpg" alt />
                <p style="color: #14679f; font-size: 14px">
                  国泰达鸣精密机件（深圳）有限公司
                </p>
              </li>
              <li>
                <img src="../assets/img/img3.jpg" alt />
                <p style="color: #14679f; font-size: 14px">
                  丽晶维珍妮内衣（深圳）有限公司
                </p>
              </li>
              <li>
                <img src="../assets/img/img4.jpg" alt />
                <p style="color: #14679f; font-size: 14px">
                  深圳大学附属实验中学
                </p>
              </li>
              <li>
                <img src="../assets/img/img5.png" alt />
                <p style="color: #14679f; font-size: 14px">
                  深圳华美金属材料科技有限公司
                </p>
              </li>
              <li>
                <img src="../assets/img/img6.png" alt />
                <p style="color: #14679f; font-size: 14px">
                  深圳市场监督管局宝安监管局
                </p>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Banner from "../components/Banner";
//
export default {
  components: {
    Banner,
    // swiper,
    // swiperSlide
  },
  data() {
    return {
      loading: true,
      honorList: [],
      partnerImg: [
        {
          Id: 1,
          Img: "../assets/img/img1.jpg",
          Remark: "广东内伶仃福田国家级自然保护区管理局",
          CreateTime: "2023-11-29T13:50:15.743",
        },
        {
          Id: 2,
          Img: "../assets/img/img2.jpg",
          Remark: "国泰达鸣精密机件（深圳）有限公司",
          CreateTime: "2020-06-05T21:34:41.023",
        },
        {
          Id: 3,
          Img: "../assets/img/img3.jpg",
          Remark: "丽晶维珍妮内衣（深圳）有限公司",
          CreateTime: "2020-06-05T21:34:36.397",
        },
      ],
      courseList: [],
      teamItem: [],
      swiperOption: {
        navigation: {
          nextEl: ".swiper-button-next",
          prevEl: ".swiper-button-prev",
        },
      },
      dialogTableVisible: false,
      dialogUrl: "",
      dialogTitle: "",
    };
  },
  mounted() {
    this.$http
      .all([
        this.$http.get("honor/list"),
        // this.$http.get("Enterprise/GetEnterpriseAll"),
        // this.$http.get(`Team/GetTeamAll`),
        // this.$http.get(`Course/GetCourseAll`)
      ])
      .then(
        this.$http.spread((responseHonor) => {
          //responseEnterprise, responseTeam, responseCourse
          this.honorList = responseHonor.data.data;
          for (let obj of this.honorList) {
            obj.filePath = this.imgserver + obj.filePath;
          }

          // this.partnerImg = responseEnterprise.data;
          // this.teamItem = responseTeam.data;

          // var groupCount = Math.ceil(responseCourse.data.length / 2);
          // for (let i = 0; i < groupCount; i++) {
          //   let img2 = [];
          //   for (let j = 0; j < 2; j++) {
          //     if (responseCourse.data.length - 1 >= i * 2 + j) {
          //       img2.push(responseCourse.data[i * 2 + j]);
          //     }
          //   }
          //   this.courseList.push(img2);
          // }
          // window.console.log(this.courseList);
          this.loading = false;
        })
      );
  },
};
</script>

<style lang="scss" scoped>
* {
  margin: 0;
  padding: 0;
}

.go-in {
  width: 100%;
  height: 100%;
  background-color: #14679f;
  position: relative;
  overflow: hidden;
  .section {
    width: 100%;
    &-content {
      width: 1240px;
      margin: 0 auto;
      background-color: #fff;

      .content-summary {
        //height: 500px;
        display: flex;
        justify-content: space-around;
        padding: 100px 0;
        .summary-left {
          width: 600px;
          .title {
            font-size: 25px;
            color: #e13834;
          }
          .eTitle {
            font-size: 17px;
            color: #e13834;
            padding: 20px 0;
          }
          .content {
            color: #14679f;
            font-size: 14px;
            text-indent: 25px;
            line-height: 30px;
          }
        }

        .summary-right {
          width: 400px;
          height: 310px;
          border: 2px solid #1d42b9;
          //animation: imgboxkey 4s infinite;
          border-radius: 10px;
          margin-top: 80px;
          text-align: center;

          img {
            width: 360px;
            height: 270px;
            margin-top: 20px;
            //animation: imgbo 4s infinite;
          }
        }
      }

      //发展历程
      .content-course {
        padding: 50px 0;
        .course-time {
          width: 1000px;
          height: 400px;
          margin: 20px auto;
          .swiper-container {
            height: 100%;
          }
          .time-show {
            width: 700px;
            height: 100%;
            margin: 0 auto;
            display: flex;
            .time-show-item {
              width: 350px;
              height: 100%;
              overflow: hidden;
              display: flex;
              flex-direction: column;

              .item-top,
              .item-bottom {
                height: 190px;
              }
              .item-bottom {
                // display: flex;
                // align-content: center;
                .item-bottom-content {
                  background-color: #1667a0;
                  margin: 20px 0;
                  p {
                    color: #fff;
                    text-align: center;
                    padding: 15px;
                  }
                }
              }
            }
          }
        }
      }

      //企业文化
      .content-culture {
        height: 450px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: flex-start;
        padding-left: 100px;
        background-color: #c2daeb;
        h3 {
          color: #3c6088;
          font-size: 26px;
        }
        p {
          color: #3c6088;
          font-size: 26px;
          padding: 5px 0;
        }
        span {
          font-weight: 400;
          line-height: 36px;
          font-size: 18px;
          padding: 5px 0;
        }
      }

      //公司荣誉
      .content-honor {
        padding: 50px 0;
        .honor-show {
          width: 1000px;
          margin: 30px auto;
          display: flex;
          flex-wrap: wrap;
          justify-content: flex-start;
          align-content: flex-start;

          li {
            width: 220px;
            height: 320px;
            margin-left: 15px;
            list-style: none;
            border: 1px solid palegoldenrod;

            img {
              width: 100%;
              height: 100%;
            }
          }
        }
        p {
          text-align: center;
          color: #3c6088;
        }
      }

      //团队风采
      .content-team {
        padding: 50px 100px;
        .swiper-img {
          height: 400px;
          background: no-repeat center;
          background-size: cover;
        }
      }

      //合作伙伴
      .content-partner {
        padding: 50px 0;
        .partner-img {
          width: 950px;
          margin: 20px auto;
          display: flex;
          justify-content: flex-start;
          flex-wrap: wrap;
          li {
            width: 107px;
            height: 107px;
            list-style: none;
            margin: 10px 25px;

            img {
              width: 100%;
              height: 100%;
              border: 1px solid rgb(194, 218, 235);
            }
          }
        }
      }
    }
  }
}

@keyframes imgboxkey {
  0% {
    border: solid rgb(29, 66, 185) 2px;
  }
  40% {
    border: solid rgb(255, 255, 255) 2px;
  }
  60% {
    border: solid rgb(255, 255, 255) 2px;
  }
  100% {
    border: solid rgb(29, 66, 185) 2px;
  }
}

@keyframes imgbo {
  0% {
    transform: scale(1);
    box-shadow: 0px 0px 0px 0px #ababab;
  }
  50% {
    transform: scale(1.1);
    box-shadow: 0px 0px 10px 5px #ababab;
  }
  100% {
    transform: scale(1);
    box-shadow: 0px 0px 0px 0px #ababab;
  }
}
.el-divider--horizontal {
  margin: 1px 0;
}

.top {
  h3,
  p {
    text-align: center;
    color: #3c6088;
    font-weight: 400;
    padding: 10px 0;
  }
  h3 {
    font-size: 30px;
  }
  p {
    font-size: 20px;
  }
  .border {
    border-bottom: 1px solid #3c6088;
    width: 15%;
    margin: 0 auto;
  }
}
// .swiper-button-disabled {
//   display: none;
// }
.order {
  order: -1;
}
.order-top {
  order: 1;
}
.el-divider {
  background-color: red;
  height: 3px;
  .el-divider__text {
    width: 20px;
    height: 20px;
    border-radius: 50%;
    padding: 0px;
    color: #fff;
    border: 3px solid red;
  }
}
.el-divider-active {
  background-color: #3c6088;
}
.honor-big-img {
  width: 100%;
  height: 100%;
  z-index: 10;
  text-align: center;
  padding-bottom: 20px;
  padding: 5%;
  //background-color: #14679f;
  .el-dialog__wrapper {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    overflow: auto;
    margin: 0;

    .el-dialog__body {
      overflow: hidden;
      img {
        width: 100%;
      }
    }
  }
}
@media screen and (max-width: 768px) {
  .go-in {
    &-section {
      &-content {
        width: 90vw;

        .content-summary {
          flex-direction: column;
          align-items: center;

          .summary-left {
            width: 90%;
          }

          .summary-right {
            width: 90%;
            margin-top: 20px;
          }
        }

        .content-honor {
          .honor-show {
            justify-content: center;
          }
        }

        .content-team {
          .swiper-img {
            height: 300px;
          }
        }

        .content-partner {
          .partner-img {
            justify-content: center;
          }
        }
      }
    }
  }
}
</style>