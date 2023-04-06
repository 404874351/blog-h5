<template>
  <div style="margin-top: 80px; ">
    <img src="@/assets/image/404.png" class="error-img" >
    <div class="margin text-center animate__animated animate__slideInUp">
      <div class="text-bold text-sl text-blue margin-bottom">诶呀!</div>
      <div class="text-bold text-lg margin-bottom">您无法访问该页面!</div>
      <div class="text-gray text-df">请检查您输入的网址是否有误，或联系管理员。</div>
      <div class="margin-top-xl">
        <el-button type="primary" style="width: 90%"  @click="navigateBack">返回</el-button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: '404',
  data() {
    return {

    }
  },
  methods: {
    navigateBack() {
      this.$router.back()
    }
  }
}
</script>

<style scoped>
.error-img {
  width: 80vw;
  display: block;
  margin: 0 auto;
}
</style>