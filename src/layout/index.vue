<template>
  <div>
    <!-- 导航栏 -->
    <Header  />
    <!-- 路由页面 -->
    <router-view />
    <!-- 底部栏 -->
    <Footer  />
    <!-- 返回顶部 -->
    <BackTop />
    <!-- 登录框 -->
    <LoginDialog />
    <!-- 右侧抽屉 -->
    <RightDrawer />
    
  </div>
    
</template>

<script>
import Header from "@/components/Header";
import BackTop from "@/components/BackTop";
import Footer from "@/components/Footer";
import LoginDialog from "@/components/LoginDialog";
import RightDrawer from "@/components/RightDrawer.vue";

export default {
  name: "Layout",
  components: {
    Header,
    BackTop,
    Footer,
    LoginDialog,
    RightDrawer,
},
  data() {
    return {
      
    };
  },
  computed: {

  },
  watch: {
    $route(newVal, oldVal) {
      // 监听路由变化
    }
  },
  created() {

  },
  mounted() {
    
  },
  
  methods: {
    
  }
};
</script>

<style lang="scss" scoped>

</style>