<template>
  <div id="app">
      <Header></Header>
      <Content :getData="getData"></Content>
      <item :arr="arr" @changeArr="updateArr" :getData="getData"></item>
  </div>
</template>

<script>
  import Header from './components/Header'
  import Content from './components/Content'
  import item from './components/item'
  import axios from 'axios'
export default {
  name: 'App',
  data(){
    return{
      arr:[{
        title:"吃饭",
        isDone:false
      }]
    }
  },
  methods:{
    updateArr (val){
        this.arr = val;
    },
    getData (){
      axios.get("/api/todo").then(res=>{
        // console.log(res);
        this.arr = res.data.data.map(item=>{
          item.isShow = false;
          return item;
        })
      })
    }
  },
  mounted(){
    this.getData();
  },
  components:{
    Header,
    Content,
    item
  }
}
</script>

<style>
  *{
    padding: 0;
    margin: 0;
  }
  li{
    list-style: none;
  }
  #app {
    width: 800px;
    margin: 50px auto;
  }
</style>
