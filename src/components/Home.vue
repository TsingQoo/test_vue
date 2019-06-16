<template>
    <div>
        --{{"父组件的title"+title}}--
        --{{"子组件自己的title"+thetitle}}--
        <button @click="getParentDescrible()">通过父组件属性调用父组件方法</button>
        <button @click="getData()">通过传递的属性方法调用父组件方法</button>
        <button @click="emiteTo()">向组件广播数据</button>
        <!---->
        <v-life></v-life>
    </div>
</template>
<script>

//扩展模块
import Lify from './Lify.vue';
//广播承载组件
import VueEvent from '../model/VueEvent.js';

export default {
    data(){
        return{
            thetitle:"TodoList测试的home组件标题"
        }
    },
    methods:{
        getParentDescrible(){
            //调用父组件的函数
            this.parent.moduleDescrible();
        },
        Describle(){
            alert("这是一个组件，名字叫Home");
        },
        emiteTo(){
            //将函数“to-news”广播给其他组件 
            VueEvent.$emit('to-news',this.thetitle)
        }
    },
    mounted(){
        //接收广播（“to-home”函数在News组件中被广播）
        VueEvent.$on('to-home',function(data){
                alert("这里是home组件，接收到news组件的广播数据："+data);
            })
    },
    //注册模块
    components:{
        'v-life':Lify
    },
    props:['title','getData','parent']
}
</script>
<style scoped>
div{
    color:blueviolet;
    font-size: 20px;
}
</style>

