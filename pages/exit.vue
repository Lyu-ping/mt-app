<template>
  <div></div>
</template>

<script>
import axios from 'axios'
import { async } from 'q';
export default {
    layout:'blank',
    middleware:async (ctx)=>{
        let {status,data} = await axios.get('/users/exit')
        if(status === 200&&data&&data.code===0) {
            window.location.href = "/"
        }
    }
}
</script>

<style>

</style>