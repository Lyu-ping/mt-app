<template>
  <el-container class="layout-default">
    <el-header height="auto">
      <my-header/>
    </el-header>
    <el-main>
      <nuxt/>
    </el-main>
    <el-footer height="auto">
      <my-footer/>
    </el-footer>
  </el-container>
</template>

<script>
import MyHeader from '@/components/public/header/index.vue'
import MyFooter from '@/components/public/footer/index.vue'
export default {
  name:'HeaderIndex',
  components: {
    MyHeader,
    MyFooter
  }
}
</script>

<style>

</style>
