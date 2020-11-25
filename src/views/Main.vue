<template>
  <div class="main">
    <!-- 二级路由 -->
    <div>
      <router-view></router-view>
    </div>

    <van-tabbar
      v-model="activeName"
      inactive-color="black"
      active-color="skyblue"
      route
    >
      <van-tabbar-item
        :icon="item.icon"
        v-for="(item, index) in tabbarData"
        :key="index"
        :name="item.name"
        replace
        :to="item.path"
        >{{ item.title }}</van-tabbar-item
      >
    </van-tabbar>
  </div>
</template>

<script>
export default {
  name: "Main",
  data() {
    return {
      // 激活下标标签
      activeName: "Home",

      tabbarData: [
        {
          title: "首页",
          icon: "home-o",
          // 路由名称 路由名称
          name: "Home",
          path: "/main/home",
        },
        {
          title: "菜单",
          icon: "apps-o",
          name: "Menu",
          path: "/main/menu",
        },
        {
          title: "购物车",
          icon: "shopping-cart-o",
          name: "Shopbag",
          path: "/main/shopbag",
        },
        {
          title: "我的",
          icon: "manager-o",
          name: "My",
          path: "/main/my",
        },
      ],
    };
  },

  methods: {},
};
</script>

<style lang="less" scoped>
.main {
  padding-bottom: 50px;
}
</style>