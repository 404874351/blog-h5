<template>
  <div :class="navClass">
    <div
      style="height: 60px; "
      class="flex justify-between align-center padding-lr">
      <!-- 页面导航 -->
      <div class="flex align-center">
        <!-- 网站logo -->
        <img src="@/assets/image/logo.png" width="55" class="cursor-pointer padding-lr-sm" @click="nav({ path: '/' })" />
      </div>
      <!-- 功能按钮 -->
      <div class="flex align-center">
        <div v-if="$store.state.userInfo" class="margin-right-xs" @click="nav({ path: '/user' })">{{$store.state.userInfo.nickname}}</div>
        <div class="cursor-pointer" @click="showRightDrawer"><i class="el-icon-menu margin-left-xs text-xxl"></i></div>
      </div>
      
    </div>
  </div>
  
</template>

<script>

export default {
  name: "Header",
  props: {},
  data() {
    return {
      navClass: 'nav',
      scrollTop: 0,
    }
  },
  created() {},
  mounted() {
    // 监听页面滚动 
    window.addEventListener("scroll", this.scroll);
  },    
  
  methods: {
    /**
     * 滚动回调，用以控制导航栏样式
     * 
     * 滚动到顶部，导航栏固定且无背景
     * 向上滚动，导航栏固定且有背景
     * 向下滚动，导航栏隐藏
     */
    scroll() {
      let scrollTop =
        window.pageYOffset ||
        document.documentElement.scrollTop ||
        document.body.scrollTop
      if (scrollTop > 60) {
        this.navClass = "nav-fixed"
      } else {
        this.navClass = "nav"
      }
      if (this.scrollTop < scrollTop) {
        this.navClass += " nav-hidden"
      }
      this.scrollTop = scrollTop
    },

    showRightDrawer() {
      this.$store.commit('SET_DRAWER_VISIBLE', true)
    },

    nav(item) {
      this.$router.push({ path: item.path })
    },
    
  },
};
</script>

<style lang="css" scoped>
.nav {
  position: fixed;
  top: 0;
  z-index: 999;
  width: 100%;
  background-color: transparent;
  color: white;
  transition: all ease-in-out .4s;
}

.nav-fixed {
  position: fixed;
  top: 0;
  z-index: 999;
  width: 100%;
  background-color: #ffffffb0;
  color: #303133;
  transition: all ease-in-out .4s;
}

.nav >>> input {
  color: white;
  background-color: transparent !important;
  transition: all ease-in-out .4s;
}

.nav-fixed >>> input {
  color: initial;
  transition: all ease-in-out .4s;
}

.nav-hidden {
  transform: translateY(-100%);
}

.nav .nav-user {
  color: white;
  transition: all ease-in-out .4s;
}
.nav-fixed .nav-user {

}
</style>