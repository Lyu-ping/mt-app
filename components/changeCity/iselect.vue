<template>
  <div class="m-iselect">
    <span class="name">按省份选择:</span>
    <el-select v-model="Pvalue" placeholder="省份">
      <el-option
        v-for="(item,index) in province"
        :key="index"
        :label="item.label"
        :value="item.value"
      ></el-option>
    </el-select>
    <el-select v-model="Cvalue" placeholder="城市" :disabled="!city.length">
      <el-option v-for="(item,index) in city" :key="index" :label="item.label" :value="item.value"></el-option>
    </el-select>
    <span class="name">直接搜索：</span>
    <el-autocomplete
      v-model="input"
      :fetch-suggestions="querySearchAsync"
      placeholder="请输入城市名称"
      @select="handleSelect"
    ></el-autocomplete>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  data() {
    return {
      Pvalue: "",
      province: [],
      Cvalue: "",
      city: [],
      input: "",
      cities: []
    };
  },
  watch: {
    async Pvalue(newPvalue) {
      let {
        status,
        data: { city }
      } = await this.$axios.get(`/geo/province/${newPvalue}`);
      if (status === 200) {
        this.city = city.map(item => {
          return {
            label: item.name,
            value: item.id
          };
        });
        this.Cvalue = "";
      }
    }
  },
  async mounted() {
    let {
      status,
      data: { province }
    } = await this.$axios.get("/geo/province");
    this.province = province.map(item => {
      return {
        value: item.id,
        label: item.name
      };
    });
  },
  methods: {
    querySearchAsync: _.debounce(async function(query, cb) {
      if (this.city.length) {
        cb(this.cities.filter(item => item.value.indexOf(query) > -1))
      } else {
        let {status,data:{city}} = await this.$axios.get('/geo/city')
        if(status === 200) {
          this.cities = city.map(item => {
            return {
              value:item.name
            }
          })
          cb(this.cities.filter(item => item.value.indexOf(query) > -1))
        } else {
          cb([])
        }
      }
    }, 200),
    handleSelect() {
      window.location.href = '/'
    }
  }
};
</script>

<style lang="scss">
@import "@/assets/css/changecity/iselect.scss";
</style>
