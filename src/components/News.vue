<template>
    <div id="news">
       我是新闻组件
        <br>
       <button @click="emitHome()">给Home组件广播数据</button>
        <br>
    </div>
</template>
<script>

//扩展模块
import Lify from './Lify.vue';

//广播承载组件
import VueEvent from '../model/VueEvent.js';

export default{
    data(){
        return {               
            msg:'我是一个新闻组件'              
        }
    },
    methods:{
        emitHome(){ 
            //广播
            VueEvent.$emit('to-home',this.msg)
        }

    },
    mounted(){
        //接收广播数据（“to-news”在Home组件中被广播）
        VueEvent.$on('to-news',function(data){
            alert("这里是new是组件，接收到home组件的广播数据："+data);
        })
    }
}
</script>
<style scoped>
div{
    color:blueviolet;
    font-size: 20px;
}
</style>

