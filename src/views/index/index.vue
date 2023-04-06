<template>
  <div>
    <div ref="banner" class="banner"  style="height: 100vh;" :style="{ 'background-image': 'url(' + bannerImage + ')' }">
      <div>
        <div class="text-center text-bold margin-bottom-xl animate__animated animate__zoomIn" style="font-size: 32px;">晨曦的个人网站</div>
        <div class="typer">{{ obj.output }}<span class="typed-cursor">|</span></div>
      </div>
      <div class="scroll-down animate__animated animate__shakeY animate__infinite" @click="scrollDown">
        <i class="el-icon-arrow-down"></i>
      </div>
    </div>

    <!-- 文章信息 -->
    <div v-if="!articleList.length" v-loading="true" class="margin-tb-xl" ></div>
    <el-card 
      v-for="(item, index) in articleList" 
      :key="'article-' + index" 
      class="article-card cursor-pointer animate__animated animate__zoomIn"
      style="border-radius: 10px;">
      <div @click="navArticle(item.id)" >
        <div style="height: 200px; width: 100%; position: relative;">
          <el-image 
            style="height: 200px; width: inherit;"
            fit="cover" 
            :src="item.coverUrl || require('@/assets/image/default_cover.jpg')" lazy></el-image>
            <el-tag v-if="item.top" size="small" type="warning" class="article-tag">置顶</el-tag>
        </div>
        <div class="padding-lg">
          <div class="text-xxl text-cut" style="max-width: inherit;">{{ item.title }}</div>
          <div class="margin-tb-sm flex align-center">
            <div class="text-sm text-gray"><i class="el-icon-alarm-clock"></i> {{ item.createTime.slice(0, 10) }}</div>
            <el-divider direction="vertical"></el-divider>
            <div class="text-sm text-gray"><i class="el-icon-collection"></i> {{ item.category.name }}</div>
            <el-divider direction="vertical"></el-divider>
            <div class="text-sm text-gray"><i class="el-icon-view"></i> {{ item.viewCount }}</div>
            <el-divider direction="vertical"></el-divider>
            <div class="text-sm text-gray"><i class="el-icon-star-off"></i> {{ item.praiseCount }}</div>
          </div>
          <div class="article-description">{{ item.description }}</div>
        </div>
      </div>
    </el-card>
    
  </div>
</template>

<script>
import EasyTyper from "easy-typer-js";
import { articlePage } from "@/api/article.js";

export default {
  name: 'Index',
  data() {
    return {
      bannerImage: require('../../assets/image/bg.jpg'),

      typerContentList: [
        "弱小和无知不是生存的障碍，\n傲慢才是。——《三体》",
        "理科生不会写，文科生不会算，\n大概的确是不对的。"
      ],
      typerFinished: false,

      obj: {
				output: "",
        type: 'rollback',
				isEnd: false,
				speed: 150,
				singleBack: true,
				sleep: 10 * 1000,
				backSpeed: 150,
				sentencePause: false
			},

      articleList: [],
    }
  },
  watch: {
    typerFinished(newVal, oldVal) {
      if (newVal == true) {
        this.typerFinished = false
        this.initTyped()
      }
    }
  },
  created() {
    this.initTyped()
    this.getArticle()
  },
  
  methods: {
    scrollDown() {
      console.log(this.$refs['banner'].clientHeight);
      window.scrollTo({
        behavior: "smooth",
        top: this.$refs['banner'].clientHeight
      });
    },

    initTyped() {
			const obj = this.obj
      const input = [ ...this.typerContentList ]
      const fn = (instance) => { this.typerFinished = true }
      const hooks = (instance) =>{}
			const typed = new EasyTyper(obj, input, fn, hooks)
		},

    getArticle() {
      articlePage({
        current: 1, 
        size: 10, 
        sortBy: "article_view_count",
      }).then(res => {
        this.articleList = res.data.records
      }).catch(err => {
        console.log(err);
      })
    },

    navArticle(id) {
      this.$router.push({ path: `/article/${id}` })
    },

  }
}
</script>

<style scoped>
.scroll-down {
  position: absolute;
  bottom: 20px;
  left: 0;
  width: 100%;
  font-size: 30px;
  font-weight: bold;
  text-align: center;
  color: white;
  cursor: pointer;
}

.animate__animated.animate__shakeY {
  --animate-duration: 10s;
}

.typed-cursor {
  opacity: 1;
  animation: blink 2s infinite;
}

.typer {
  height: 80px;
  white-space: pre;
  word-break: break-all;
  line-height: 1.6;
  font-size: 18px;
}

.article-card {
  margin-top: 15px;
  margin: 10px auto;
  width: 94%;
}

.article-card:first-child {
  margin-top: 0;
}
.article-card >>> .el-card__body {
  padding: 0;
}

.article-tag {
  position: absolute;
  left: 10px;
  top: 10px;
}

.article-description {
  min-height: 50px;
  font-size: 14px;
  line-height: 1.8;
  word-break: break-all;
  overflow:hidden; 
  text-overflow:ellipsis;
  display:-webkit-box;    
  -webkit-box-orient:vertical; 
  -webkit-line-clamp:2; 
}
</style>