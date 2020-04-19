<template>
  <el-container>
    <el-header class="myHeader">
      <slot></slot>
    </el-header>
  </el-container>
</template>

<script>
export default {

}
</script>

<style scoped>
.myHeader{
  /* background-color:#2366D3; */
  /* color:#fff; */
  font-size: 2rem;
  padding-top: 3rem;
  padding-bottom: 3rem;
}
</style>
