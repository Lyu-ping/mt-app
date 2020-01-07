<template>
  <div class="page-index">
    <el-row>
      <el-col :span="5">
        <my-menu />
      </el-col>
      <el-col :span="19">
        <my-life/>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="24">
        <my-artistic/>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import MyMenu from "../components/index/menu";
import MyLife from '../components/index/life';
import MyArtistic from '../components/index/artistic'
export default {
  components: {
    MyMenu,
    MyLife,
    MyArtistic
  }
};
</script>

<style lang="scss">
@import "@/assets/css/index/index.scss";
</style>
