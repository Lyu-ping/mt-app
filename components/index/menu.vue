<template>
  <div class="m-menu">
      <dl class="nav" @mouseleave="leave">
          <dt>全部分类</dt>
          <dd v-for="(item, index) in $store.state.home.menu" @mouseenter="enter">
            <i :class="item.type" />{{item.name}}<span class="arrow"></span>
          </dd>
      </dl>
      <div class="detail" v-if="kind" @mouseenter="detailEnter" @mouseleave="detailLeave">
          <template v-for="(item, index) in curdetail.child">
            <h4 :key="index">{{item.title}}</h4>
            <span v-for="v in item.child" :key="v">{{v}}</span>
          </template>
      </div>
  </div>
</template>

<script>
export default {
    data() {
        return {
            kind:'',
            menu:[{
                type:'food',
                name:'美食',
                child:[{
                    title:'美食',
                    child:['火锅','西餐','烧烤','甜点']
                }]
            },{
                type:'takeout',
                name:'外卖',
                child:[{
                    title:'外卖',
                    child:['美团外卖']
                }]
            },{
                type:'hotel',
                name:'酒店',
                child:[{
                    title:'酒店',
                    child:['火锅','西餐','烧烤','甜点']
                }]
            }]
        }
    },
    computed:{
        curdetail() {
            return this.$store.state.home.menu.filter( item => item.type===this.kind)[0]
        }
    },
    methods:{
        enter(e) {
            this.kind = e.target.querySelector('i').className
        },
        leave() {
            this.timer=setTimeout(() => {
                this.kind = ''
            }, 150);
        },
        detailEnter() {
            clearTimeout(this.timer)
        },
        detailLeave() {
            this.kind = ''
        }
    }
}
</script>

<style>

</style>