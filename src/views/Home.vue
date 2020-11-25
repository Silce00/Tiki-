<template>
  <div class="home">
    <van-nav-bar>
      <template #left>
        <div class="home-nav">
          <div class="t1">欢迎回来，</div>
          <div class="t2">{{ nickName }}</div>
        </div>
      </template>
      <template #right>
        <div class="home-search">
          <van-search placeholder="请输入搜商品名称" @focus="searchFocus" />
        </div>
      </template>
    </van-nav-bar>

    <div class="home-content">
      <!-- 轮播图 -->
      <div class="banner-box">
        <van-swipe @change="changeCurrentIndex" :autoplay="5000" loop>
          <van-swipe-item v-for="(item, index) in bannerData" :key="index">
            <img
              :src="item.bannerImg"
              class="auto-img"
              alt=""
              @click="goDetail(item.pid)"
            />
          </van-swipe-item>

          <template #indicator>
            <div class="index-box">
              <div
                :class="{ active: index == currentIndex }"
                v-for="(item, index) in bannerData"
                :key="index"
              ></div>
            </div>
          </template>
        </van-swipe>
      </div>
    </div>

    <div class="product-box">
      <div>
        <div class="clearfix pro-title-box">
          <div class="pro-title">爆款商品</div>
        </div>
        <div class="products clearfix">
          <div
            class="pro-item fl"
            v-for="(item, index) in hotProduct"
            :key="index"
            @click="goDetail(item.pid)"
          >
            <div>
              <img class="auto-img" :src="item.smallImg" />
              <!-- hot标签 -->
              <!-- <div class="hot">hot</div> -->
            </div>
            <div class="pro-info">
              <div class="pro-name one-text">{{ item.name }}</div>
              <div class="pro-enname one-text">{{ item.enname }}</div>
              <div class="pro-price"><span>￥</span>{{ item.price }}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import "../assets/less/home.less";
export default {
  name: "Home",
  data() {
    return {
      nickName: "",

      // 当前图片索引
      currentIndex: 0,

      // 轮播数据
      bannerData: [],

      // 热卖商品数据
      hotProduct: [],
    };
  },
  created() {
    // 获取轮播数据
    this.getBannerData();

    // 获取热卖商品
    this.getHotProduct();

    this.getuserName();
  },

  methods: {
    // 修改轮播图片索引
    changeCurrentIndex(index) {
      
      this.currentIndex = index;
    },
    // 获取轮播图片数据
    getBannerData() {
      this.$toast.loading({
        // 文本提示
        message: "加载中...",
        // 禁止穿透点击
        forbidClick: true,
        // 提示时间  没有时间限制  需要手动关闭
        duration: 0,
      });

      //发起注册请求
      this.axios({
        //请求类型
        method: "GET",
        //请求路径
        url: "/banner",

        //POST请求参数, object
        params: {
          appkey: this.appkey,
        },
      })
        .then((result) => {
          //关闭加载提示
          this.$toast.clear();

          

          if (result.data.code == 300) {
            
            this.bannerData = result.data.result;
          }
        })
        .catch((err) => {
          this.$toast.clear();

          
        });
    },
    getHotProduct() {
      this.$toast.loading({
        // 文本提示
        message: "加载中...",
        // 禁止穿透点击
        forbidClick: true,
        // 提示时间  没有时间限制  需要手动关闭
        duration: 0,
      });

      //发起注册请求
      this.axios({
        //请求类型
        method: "GET",
        //请求路径
        url: "/typeProducts",

        //POST请求参数, object
        params: {
          appkey: this.appkey,
          key: "isHot",
          value: 1,
        },
      })
        .then((result) => {
          //关闭加载提示
          this.$toast.clear();

          
          if (result.data.code == 500) {
            this.hotProduct = result.data.result;
            
          }
        })
        .catch((err) => {
          this.$toast.clear();

          
        });
    },

    // 查看商品详细页面
    goDetail(pid) {
      
      this.$router.push({ name: "Detail", params: { pid } });
    },
    //获取用户信息
    getuserName() {
      let tokenString = localStorage.getItem("_tk");

      if (!tokenString) {
        return;
      }

      this.$toast.loading({
        message: "加载中...",
        forbidClick: true,
        duration: 0,
      });

      this.axios({
        method: "GET",
        url: "/findAccountInfo",
        params: {
          appkey: this.appkey,
          tokenString,
        },
      })
        .then((result) => {
          this.$toast.clear();
          

          if (result.data.code == 700) {
            //token检验无效,则跳到登录页面
            this.$router.push({ name: "Login" });
          } else if (result.data.code == "B001") {
            this.nickName = result.data.result[0].nickName;
          }
        })
        .catch((err) => {
          this.$toast.clear();
          
        });
    },

    //搜索栏获取焦点
    searchFocus() {
      this.$router.push({ name: "Search" });
    },
  },
};
</script>

<style lang="less" scoped>
</style>