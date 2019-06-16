<template>
  <div id="app">
    <v-home :title="title" :getData="moduleDescrible" :parent="this" ref="home"></v-home>
    <v-news></v-news>
    <input type="text" v-model="todo" @keydown="DoAdd($event)">
    <hr>
    <br>

    <h2>进行中</h2>
    <ul>
      <li v-for="(item,key) in list" v-if="!item.checked">
        <input type="checkbox" v-model="item.checked" @change="savelist()">
        ---{{item.title}}---
        <button @click="remove(key)">删除</button>
      </li>
    </ul>

    <h2>已完成</h2>
    <ul>
      <li v-for="(item,key) in list" v-if="item.checked">
        <input type="checkbox" v-model="item.checked" @change="savelist()">
        ---{{item.title}}---
        <button @click="remove(key)">删除</button>
      </li>
    </ul>

    <hr>
    <br>
    <button @click="getData()">请求数据</button>
    <button @click="getChil()">获取子组件Home的属性和方法</button>
    <ul>
      <li v-for="item in list1">{{item.title}}</li>
    </ul>
  </div>
</template>
<script>
//引入组件
import Home from "./components/Home.vue";
import News from "./components/News.vue";
//js组件
import storage from "./model/storage.js";

export default {
  name: "App",
  data() {
    return {
      title: "TodoList",
      todo: "",
      list: [],
      list1: []
    };
  },
  methods: {
    moduleDescrible() {
      alert("这是一个组件，名字叫app");
    },
    DoAdd(e) {
      //enter键
      if (e.keyCode == 13) {
        var info = {
          title: this.todo,
          checked: false
        };
        this.list.push(info);
        //使用js组件的方法
        storage.set("list", this.list);
      }
    },
    savelist() {
      storage.set("list", this.list);
    },
    remove(key) {
      this.list.splice(key, 1);
      storage.set("list", this.list);
    },
    getData() {
      var api ="http://www.phonegap100.com/appapi.php?a=getPortalList&catid=20&page=1";
      this.$http.get(api).then(
        response => {
          console.log(response);
          //注意this指向
          this.list1 = response.body.result;
        },
        function(err) {
          console.log(err);
        }
      );
    },
    getChil() {
      this.$refs.home.Describle();
      alert("子组件的title：" + this.$refs.home.thetitle);
    }
  },
  //*请求数据，操作dom , 放在这个里面  mounted*/
  mounted() {
    this.getData();
    var list = storage.get("list");
    if (list) {
      this.list = list;
    }
  },
  beforeCreate() {
    console.log("实例刚刚被创建1");
  },
  created() {
    this.getData();
    console.log("实例已经创建完成2");
  },
  beforeMount() {
    console.log("模板编译之前3");
  },
  beforeUpdate() {
    console.log("数据更新之前");
  },
  updated() {
    console.log("数据更新完毕");
  },
  beforeDestroy() {
    /*页面销毁的时候要保存一些数据，就可以监听这个销毁的生命周期函数*/
    console.log("实例销毁之前");
  },
  destroyed() {
    console.log("实例销毁完成");
  },
  //挂载组件
  components: {
    "v-home": Home,
    "v-news": News
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
