<template>
  <div class="NewsDetails">
    <banner img="../assets/img/bgtop.jpg" />
    <div class="NewsDetails-product">
      <div class="NewsDetails-product-content">
        <img :src="newsIdList.filePath" alt />
        <p class="product-title">{{newsIdList.title}}</p>
        <p class="product-content">{{newsIdList.content}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import Banner from "../components/Banner";
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
    window.console.log(this.item);
  },
  mounted() {
    this.loadData();
  },
  methods: {
    loadData() {
          this.newsIdList = this.item;
    }
  }
};
</script>

<style lang="scss" scoped>
.NewsDetails {
  width: 100%;
  height: 100%;
  //overflow: hidden;
  background-color: #14679f;

  &-product {
    width: 80vw; 
    margin: 0 auto;
    background-color: #fff;
    border: 1px solid red;

    &-content {
      width: 60vw; 
      margin: 0 auto;
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      padding: 5vh 0; 
      img {
        width: 100%;
        height: auto;
      }

     .product-title {
        font-size: 3vw; 
        color: #e13834;
        padding: 2vh 0; 
      }

     .product-content {
        font-size: 2 vw; 
        padding: 2vh 0; 
      }
    }
  }
}

// 媒体查询，针对不同屏幕尺寸进行样式调整
@media screen and (max-width: 768px) {
.NewsDetails {
    &-product {
      &-content {
        img {
          height: 40vh; 
        }

       .product-title {
          font-size: 4vw; 
        }

       .product-content {
          font-size: 2.5vw; 
          padding: 1.5vh 0; 
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
          height: 45vh; 
        }

       .product-title {
          font-size: 3.5vw; 
          padding: 2vh 0; 
        }

       .product-content {
          font-size: 2.2vw; 
          padding: 2vh 0; 
        }
      }
    }
  }
}


</style>