<template>
  <div>
    <div class="banner" :style="{ 'background-image': 'url(' + bannerImage + ')' }">
      <div class="animate__animated animate__zoomIn">
        <div class="text-center text-bold margin-bottom-xl " style="font-size: 32px;">文章</div>
        <div>共计{{total}}条数据</div>
      </div> 
    </div>

    <el-card class="article-container animate__animated animate__zoomIn">
      <!-- 搜索栏 -->
      <div class="margin-bottom flex justify-center align-center">
        <el-input 
          v-model="key" 
          prefix-icon="el-icon-search" 
          size="medium"
          placeholder="搜索文章信息" >
        </el-input>
        <div class="margin-left">
          <el-button type="primary" size="medium" @click="getData()">搜索</el-button>
        </div>
      </div>
      
      <!-- 文章 -->
      <el-empty v-if="!list.length" description="没有找到相关信息..."></el-empty>
      <div v-loading="loading">
        <el-card 
          v-for="(item, index) in list" 
          :key="'article-' + index" 
          class="article-card cursor-pointer animate__animated animate__zoomIn"
          style="border-radius: 10px;">
          <div @click="navArticle(item.id)" >
            <div style="height: 180px; width: 100%; position: relative;">
              <el-image 
                style="height: 180px; width: inherit;"
                fit="cover" 
                :src="item.coverUrl || require('@/assets/image/default_cover.jpg')" lazy></el-image>
                <el-tag v-if="item.top" size="small" type="warning" class="article-tag">置顶</el-tag>
            </div>
            <div class="padding-lg">
              <div class="text-xxl text-cut" style="max-width: inherit;">{{ item.title }}</div>
              <div class="margin-tb-sm flex align-center">
                <div class="text-sm text-gray"><i class="el-icon-alarm-clock"></i> {{ item.createTime.slice(0, 10) }}</div>
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
      <!-- 分页 -->
      <div class="text-center margin-top">
        <el-pagination
          background
          layout="prev, pager, next"
          :total="total"
          :page-sizes="sizeList"
          :page-size="size"
          :current-page="current"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange">
        </el-pagination>
      </div>

    </el-card>
  </div>
</template>

<script>
import PageMixin from '@/mixins/PageMixin.vue'
import { articlePage } from '@/api/article';

export default {
  name: 'Article',
  mixins: [PageMixin],
  data() {
    return {
      bannerImage: require('../../assets/image/article.jpg'),
      loading: false,
      key: '',

    }
  },
  created() {
    this.key = this.$route.query.key || ''
    this.getData()
  },
  methods: {
    getData() {
      this.loading = true
      articlePage({
        current: this.current, 
        size: this.size, 
        key: this.key, 
      }).then(res => {
        this.setPage(res.data)
        this.loading = false
      }).catch(err => {
        console.log(err);
        this.loading = false
      })
    },

    navArticle(id) {
      this.$router.push({ path: `/article/${id}` })
    },
  }
}
</script>

<style scoped>
.article-container {
  margin: 10px auto;
  width: 94%;
  max-width: 1200px;
  font-size: 14px;
}

.layout {
  cursor: pointer;
  transition: all 0.2s linear;
}

.layout-selected {
  color: #409eff;
  cursor: pointer;
}

.article-card {
  margin-top: 15px;
  margin: 10px auto;
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


.article-btn {
  padding: 5px 10px;
  color: #909399;
}

.article-btn:hover {
  color: #409eff;
}

.article-btn-selected {
  padding: 5px 10px;
  color: white;
  background-color: #409eff;
}


</style>