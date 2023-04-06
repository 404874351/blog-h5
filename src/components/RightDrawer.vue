<template>
  <el-drawer
    :visible.sync="$store.state.drawerVisible"
    :with-header="false"
    size="70%" >
    <!-- 网站信息 -->  
    <el-card >
      <div class="text-center">
        <el-avatar style="width: 90px; height: 90px;" src="https://qiniu.zhongjiachen.cn/avatar/ddfd0d3d-2538-4354-b2f3-cfc686745c1f.jpg" ></el-avatar>
        <div class="text-xxl margin-tb-sm">晨曦</div>
      </div>
      <div class="flex justify-center">
        <img @click="contact('qq')"      class="contact-icon" :src="require('@/assets/image/qq.svg')"/>
        <img @click="contact('message')" class="contact-icon" :src="require('@/assets/image/message.svg')"/>
        <img @click="contact('github')"  class="contact-icon" :src="require('@/assets/image/github.svg')"/>
        <img @click="contact('gitee')"   class="contact-icon" :src="require('@/assets/image/gitee.svg')" style="scale: 0.9;" />
      </div>
      <el-row>
        <el-col :span="8">
          <div class="text-center">
            <div class="text-xxl text-black margin-tb">
              <ICountUp :endVal="articleCount"/>
            </div>
            <div class="text-df">文章</div>
          </div>
        </el-col>
        <el-col :span="8">
          <div class="text-center">
            <div class="text-xxl text-black margin-tb">
              <ICountUp :endVal="praiseCount"/>
            </div>
            <div class="text-df">点赞</div>
          </div>
        </el-col>
        <el-col :span="8">
          <div class="text-center">
            <div class="text-xxl text-black margin-tb">
              <ICountUp :endVal="commentCount"/>
            </div>
            <div class="text-df">评论</div>
          </div>
        </el-col>
      </el-row>
    </el-card>
    <div 
      v-for="(item, index) in navList" 
      :key="'nav-' + index" 
      class="nav-item"
      @click="nav(item)">
      <i :class="item.icon" class="margin-lr-sm"></i> {{ item.name }}
    </div>
    <div v-if="$store.state.userInfo"> 
      <div  class="nav-item" @click="navIndividual">
        <i class="el-icon-user-solid margin-lr-sm"></i> 个人中心
      </div>
      <div  class="nav-item" @click="userLogout">
        <i class="el-icon-remove margin-lr-sm"></i> 退出
      </div>
    </div>
    <div v-else class="nav-item" @click="showLoginDialog">
      <i class="el-icon-user-solid margin-lr-sm"></i> 登录
    </div>
  </el-drawer>
</template>

<script>
import ICountUp from 'vue-countup-v2';
import { logout } from "@/api/user.js";
import { articleStatistic } from "@/api/article.js";
import { clearLoginStatus } from "@/utils/auth.js";

export default {
  name: "RightDrawer",
  components: {
    ICountUp
  },
  data() {
    return {
      navList: [
        {
          icon: "el-icon-s-home",
          name: "首页",
          path: "/"
        },
        {
          icon: "el-icon-menu",
          name: "文章",
          path: "/article"
        },
        {
          icon: "el-icon-s-comment",
          name: "留言",
          path: "/message"
        },
        {
          icon: "el-icon-s-promotion",
          name: "关于",
          path: "/about"
        },
      ],

      commentCount: 0,
      articleCount: 0,
      praiseCount: 0,
      viewCount: 0,
    }
  },
  created() {
    this.getStatistic()
  },
  methods: {
    contact(type) {
      if (type == 'qq') {
        navigator.clipboard.writeText("404874351")
        this.$message({
          message: 'qq已复制到剪贴板',
          type: 'success',
        })
        return
      }
      if (type == 'message') {
        navigator.clipboard.writeText("jiachen_zhong@163.com")
        this.$message({
          message: '邮箱已复制到剪贴板',
          type: 'success',
        })
        return
      }
      if (type == 'github') {
        window.open('https://github.com/404874351')
        return
      }
      if (type == 'gitee') {
        window.open('https://gitee.com/zhong_jiachen')
        return
      }
    },

    getStatistic() {
      articleStatistic().then(res => {
        this.articleCount = res.data.count
        this.praiseCount = res.data.praiseCount
        this.commentCount = res.data.commentCount
        this.viewCount = res.data.viewCount
      }).catch(err => {
        console.log(err);
      })
    },

    nav(item) {
      this.$router.push({ path: item.path })
      this.$store.commit("SET_DRAWER_VISIBLE", false)
    },

    /**
     * 退出登录
     */
    userLogout() {
      const loading = this.$loading({
        lock: true,
        text: '正在退出登录',
        spinner: 'el-icon-loading',
        background: 'rgba(0, 0, 0, 0.5)'
      });

      logout({}).then(res => {
        // 清除登录态
        clearLoginStatus()
        loading.close()
        this.$store.commit("SET_DRAWER_VISIBLE", false)
      }).catch(err => {
        console.log(err);
      })
    },

    /**
     * 跳转个人页
     */
    navIndividual() {
      this.$router.push({ path: '/user' })
      this.$store.commit("SET_DRAWER_VISIBLE", false)
    },

    showLoginDialog() {
      this.$store.commit('SET_LOGIN_VISIBLE', true)
    },

  }

}
</script>

<style scoped>
.contact-icon {
  width: 30px;
  height: 30px;
  margin: 0 5px;
  cursor: pointer;
}

.nav-item {
  padding: 20px 40px;
}

</style>