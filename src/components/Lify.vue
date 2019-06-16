<template>
    <div id="life">
        生命周期
    </div>
</template>
<script>
export default {
    data(){
        return{
            msg:'生命周期'
        }
    },
    methods:{
        setMsg(){
            this.msg="生命周期";
        }
    },
    beforeCreate(){
        console.log('实例刚刚被创建1');
    },
    created(){
        console.log('实例已经创建完成2');
    },
    beforeMount(){
        console.log('模板编译之前3');
    },
    mounted(){     /*请求数据，操作dom , 放在这个里面  mounted*/
        console.log('模板编译完成4');
    },
    beforeUpdate(){
        console.log('数据更新之前');
    },
    updated(){
        console.log('数据更新完毕');
    },
    beforeDestroy(){   /*页面销毁的时候要保存一些数据，就可以监听这个销毁的生命周期函数*/
        console.log('实例销毁之前');
    },
    destroyed(){
        console.log('实例销毁完成');
    }
}
</script>
