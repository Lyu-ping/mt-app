<template>
  <div class="page-changeCity">
    <el-row>
      <el-col :span="24">
        <iSelect />
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="24">
        <Hots/>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="24">
        <cate-groy/>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import iSelect from "@/components/changeCity/iselect.vue";
import Hots from "@/components/changeCity/hot.vue"
import CateGroy from "@/components/changeCity/categroy.vue"
export default {
  components: {
    iSelect,
    Hots,
    CateGroy
  }
};
</script>

<style>
</style>