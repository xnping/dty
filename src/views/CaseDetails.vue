<template>
  <div class="case">
    <banner img="../assets/img/bgtop.jpg" />
    <div class="case-product">
      <div class="case-product-content">
        <img :src="caseIdList.filePath" alt />
        
        <p class="product-title">{{caseIdList.name}}</p>
        <p class="product-content">{{caseIdList.description}}</p>
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
      caseIdList: {},
      id:0
    };
  },
  created() {
    this.id = this.$route.params.id;
    window.console.log(this.cas.filePath);
  },  
  mounted() {
    this.loadData();
  },
  methods: {
    loadData(){
      
    }
  }
};
</script>

<style lang="scss" scoped>
.case {
  width: 100%;
  height: 100%;
  overflow: hidden;
  background-color: #14679f;

  &-product {
    width: 90vw; // 改为相对单位，占屏幕宽度的90%
    margin: 0 auto;
    background-color: #fff;
    //border: 1px solid red;

    &-content {
      width: 70vw; // 改为相对单位，占父元素宽度的80%
      margin: 0 auto;
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      padding: 5vh 0; // 改为相对单位，上下内边距为屏幕高度的5%

      img {
        width: 100%;
        height: 30vh; // 改为相对单位，占屏幕高度的45%
      }

     .product-title {
        font-size: 3vw; // 改为相对单位，根据屏幕宽度调整字体大小
        color: #e13834;
        padding: 2vh 0; // 改为相对单位，上下内边距为屏幕高度的2%
      }

     .product-content {
        font-size: 2vw; // 改为相对单位，根据屏幕宽度调整字体大小
        font-weight: bolder;
        padding: 2vh 0; // 改为相对单位，上下内边距为屏幕高度的2%
      }
    }
  }
}

// 媒体查询，针对不同屏幕尺寸进行样式调整
@media screen and (max-width: 768px) {
 .case {
    &-product {
      &-content {
        img {
          height: 40vh; // 在小屏幕下进一步调整图片高度占比，占屏幕高度的40%
        }

       .product-title {
          font-size: 4vw; // 在小屏幕下进一步调整标题字体大小，根据屏幕宽度调整字体大小
          padding: 1.5vh 0; // 在小屏幕下进一步调整上下内边距，占屏幕高度的1.5%
        }

       .product-content {
          font-size: 2.5vw; // 在小屏幕下进一步调整内容字体大小，根据屏幕宽度调整字体大小
          padding: 1.5vh 0; // 在小屏幕下进一步调整上下内边距，占屏幕高度的1.5%
        }
      }
    }
  }
}

@media screen and (min-width: 769px) and (max-width: 1024px) {
 .case {
    &-product {
      &-content {
        img {
          height: 43vh; // 在中等屏幕下调整图片高度占比，占屏幕高度的43%
        }

       .product-title {
          font-size: 3.5vw; // 在中等屏幕下调整标题字体大小，根据屏幕宽度调整字体大小
          padding: 2vh 0; // 在中等屏幕下调整上下内边距，占屏幕高度的2%
        }

       .product-content {
          font-size: 2.2vw; // 在中等屏幕下调整内容字体大小，根据屏幕宽度调整字体大小
          padding: 2vh 0; // 在中等屏幕下调整上下内边距，占屏幕高度的2%
        }
      }
    }
  }
}

@media screen and (min-width: 1025px) {
 .case {
    &-product {
      &-content {
        img {
          height: 45vh; // 在大屏幕下恢复原设置或根据需要微调
        }

       .product-title {
          font-size: 3vw; // 在大屏幕下恢复原设置或根据需要微调
          padding: 2vh 0; // 在大屏幕下恢复原设置或根据需要微调
        }

       .product-content {
          font-size: 2vw; // 在大屏幕下恢复原设置或根据需要微调
          padding: 2vh 0; // 在大屏幕下恢复原设置或根据需要微调
        }
      }
    }
  }
}
</style>