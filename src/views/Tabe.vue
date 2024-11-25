<template>
    <div class="case">
      <banner img="../assets/img/bgtop.jpg" title="业务详情" />
      <div class="case-section" v-loading="loading">
        <div class="case-section-content">
          <div class="case-section-content-list" v-for="(cas,index) in caseList" :key="index">
            <img v-lazy="cas.fileUrls" />
            <div class="content-list-abstract" :class="{'abstract-active' : index%2!=1}">
              <p class="abstract-title">{{cas.name}}</p>
              <p class="abstract-content">{{cas.description}}</p>
              <div class="more">
                <router-link
                  class="text-decoration"
                  :to="{ name: 'casedetails', params: { cas: cas.id }}"
                >
                  <span>more</span>
                  <img src="../assets/img/sanjiao.png" />
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
    components: {
      Banner
    },
    data() {
      return {
        loading: true,
        id:0,
        caseList: []
      };
    },
    created(){
      this.id = this.$route.query.id;
      window.console.log(this.id);
    },
    mounted() {
      this.$http
        .get(`case/getCases?id=${this.id}`)
        .then(response => {
          this.caseList = response.data.data;
          for(let obj of this.caseList){
            obj.fileUrls=this.imgserver+obj.fileUrls.split(",")[0]
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
        width: 70vw; // 改为相对单位，占屏幕宽度的90%
        min-height: 50vh; // 改为相对单位，最小高度为屏幕高度的50%
        margin: 0 auto;
        background-color: #fff;

        &-list {
          width: 100%;
          height: 40vh; // 改为相对单位，占屏幕高度的50%
          display: flex;
          justify-content: center;
          align-items: center;
          border: 1px solid pink;

          img {
            width: 30vw; // 改为相对单位，占屏幕宽度的50%
            height: 30vh; // 改为相对单位，占屏幕高度的30%
          }
        .content-list-abstract {
            width: 30vw; // 改为相对单位，占屏幕宽度的30%
            height: 35vh; // 改为相对单位，占屏幕高度的35%
            padding: 2vh 2vw; // 改为相对单位，内边距上下为屏幕高度的2%，左右为屏幕宽度的2%
            display: flex;
            flex-direction: column;
            justify-content: space-around;

          .abstract-title {
              line-height: 3vw; // 改为相对单位，根据屏幕宽度调整行高
              font-size: 2vw; // 改为相对单位，根据屏幕宽度调整字体大小
              color: #e13834;
            }
          .abstract-content {
              height: 20vh; // 改为相对单位，占屏幕高度的20%
              color: #484848;
              font-size: 1vw; // 改为相对单位，根据屏幕宽度调整字体大小

              overflow: hidden;
              text-overflow: ellipsis;
              display: -webkit-box;
              -webkit-line-clamp: 7;
              -webkit-box-orient: vertical;
              white-space: normal!important;
              word-wrap: break-word;
            }
          .more {
              display: flex;
              justify-content: flex-start;

            .text-decoration {
                text-decoration: none;

                span {
                  color: #000;
                }
                img {
                  width: 1vw; // 改为相对单位，根据屏幕宽度调整大小
                  height: 1vw;
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
            img {
              width: 30vw; // 在小屏幕下进一步调整图片宽度占比，占屏幕宽度的80%
              height: 25vh; // 在小屏幕下进一步调整图片高度占比，占屏幕高度的25%
            }
          .content-list-abstract {
            .abstract-title {
                line-height: 5vw; // 在小屏幕下进一步调整行高，根据屏幕宽度调整行高
                font-size: 20vw; // 在小屏幕下进一步调整字体大小，根据屏幕宽度调整字体大小
              }
            .abstract-content {
                font-size: 2.5vw; // 在小屏幕下进一步调整字体大小，根据屏幕宽度调整字体大小
              }
            .more {
              .text-decoration {
                  img {
                    width: 2vw; // 在小屏幕下进一步调整图片大小，根据屏幕宽度调整大小
                    height: 4vw;
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