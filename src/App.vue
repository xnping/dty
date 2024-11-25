<template>
  <div id="app">
    <el-container>
      <el-header>
        <div class="logo">
          <img src="./assets/img/toplogo.png" alt />
        </div>
        <el-menu
          :default-active="defaultActive"
          class="el-menu-demo"
          mode="horizontal"
          @select="handleSelect"
          :router="router"
        >
          <el-menu-item index="/">首页</el-menu-item>
          <el-menu-item index="/news">新闻资讯</el-menu-item>
          <el-menu-item index="/product">业务介绍</el-menu-item>
          <!-- <el-menu-item index="/case">经典案例</el-menu-item> -->
          <el-menu-item index="/goin">走进达通源</el-menu-item>
        </el-menu>
      </el-header>
      <el-main>
        <router-view />
      </el-main>
      <div class="footer" v-show="isShow">
        <div class="footer-content">
          <ul class="content-nav">
            <li>
              <p>走进达通源</p>
              <span>企业文化</span>
              <span>资质荣誉</span>
              <span>合作伙伴</span>
            </li>
            <li>
              <p>新闻资讯</p>
              <span>公司新闻</span>
              <span>行业动态</span>
            </li>
            <li>
              <p>联系我们</p>
              <span>电话：袁经理   0755-88608260    19842675889</span>
              <span>地址：深圳市光明区公明街道上村社区长春北路1A栋301</span>
            </li>
          </ul>
          <img src="./assets/img/ercode.png" alt />
          <img src="./assets/img/ercode.png" alt />
        </div>
      </div>
    </el-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      router: true,
      defaultActive: "/",
      isShow: false
    };
  },
  methods: {
    handleSelect(key) {
      this.isShow = this.defaultActive!= key;
      window.console.log(this.isShow);
    }
  }
};
</script>

<style lang="scss">
* {
  padding: 0;
  margin: 0;
}
html,
body {
  height: 100%;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.el-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  margin: 0 auto;

.logo {
    width: 300px;
    padding: 10px;
    img {
      width: 100%;
      height: auto;
      max-width: 100%;
    }
  }
}

.el-main {
  padding: 0 !important;
}

.footer {
  width: 100%;
  height: auto;
  background-color: #14679f;

.footer-content {
    width: 90%;
    max-width: 1200px;
    margin: 0 auto;
    padding-top: 20px;
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;

 .content-nav {
      display: flex;
      justify-content: space-around;
      flex-wrap: wrap;
      li {
        display: flex;
        flex-direction: column;
        padding: 0 20px;
        align-items: flex-start;
        p {
          font-size: 16px;
          color: #d4edff;
          padding: 10px 0;
        }
        span {
          color: #f7f7f7;
          font-weight: 300;
          padding: 5px 0;
        }
      }
    }
    img {
      width: 150px;
      height: 150px;
      padding: 10px;
      height: auto;
      max-width: 100%;
    }
  }

.copyright {
    height: 30px;
    background: #125688;
    span {
      color: #fff;
      line-height: 30px;
    }
  }

  // 媒体查询，针对小屏幕进行样式调整
  @media screen and (max-width: 768px) {
   .el-header {
        flex-direction: column;
     .logo {
          width: 100px;
          margin-bottom: 10px;
        }
      }

   .footer-content {
     .content-nav {
          flex-direction: column;
          li {
            align-items: center;
          }
        }
        img {
          margin: 0 auto;
        }
      }

    // 菜单自适应优化部分
   .el-menu-demo {
      // 当屏幕小于等于768px时，切换为垂直模式
      @media screen and (max-width: 768px) {
        mode: vertical;
        height: 5.556rem;
        width: 100%;
        border: none;
        background-color: transparent;
       .el-menu-item {
          padding: 10px 0;
          font-size: 14px;
        }
      }

      // 当屏幕在769px到1024px之间时，适当缩小菜单项间距
      @media screen and (min-width: 769px) and (max-width: 1024px) {
       .el-menu-item {
        height: 5.556rem;
          padding: 5px 0;
        }
      }

      // 当屏幕大于1024px时，恢复默认样式或可根据需求微调
      @media screen and (min-width: 1024px) {
       .el-menu-item {
        height: 5.556rem;
          padding: 8px 0;
          font-size: 16px;
        }
      }
    }
  }
}
</style>