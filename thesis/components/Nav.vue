<template>
  <el-menu
    class="el-navigation"
    mode="horizontal"
    :default-active="activeIndex"
  >
    <el-menu-item><nuxt-link to="/about">About</nuxt-link></el-menu-item>
  </el-menu>
</template>

<script>
export default {
  name: 'Nav',
  data(){
    return {
      activeIndex: '1'
    }
  }
}
</script>

<style scoped>
.el-navigation >>> a{
  text-decoration:none;
}
</style>
