<template>
<div class="content-box">
  <input type="text" placeholder="请输入要做的事..." v-model="title">
  <button @click="handleAdd">添加</button>
</div>
</template>

<script>
  import axios from 'axios'
    export default {
      props:{
          getData:{
            type:Function
          }
      },
      data(){
        return{
            title:""
        }
      },
      methods:{
        handleAdd (){
            if (!this.title.trim()) {
              alert("请输入内容！")
            }
            else{
              axios.post(`/api/todo`,{title:this.title}).then(res=>{
                  if (res.data.code == 200){
                    alert("添加成功了！")
                    this.getData();
                    this.title = ""
                  }
                  else {
                    alert("添加失败了！")
                  }
              })
            }
        }
      }
    }
</script>

<style scoped>
.content-box{
  width: 800px;
  margin-top: 20px;
  margin-bottom: 20px;
}
  input{
    width: 180px;
    height: 48px;
    line-height: 48px;
    outline: none;
    border: 1px solid skyblue;
    border-radius: 4px;
    font-size: 16px;
  }
  button{
    width: 80px;
    height: 50px;
    line-height: 50px;
    background: blue;
    color: #fff;
    border-radius: 4px;
    font-size: 20px;
    border: none;
    margin-left: 20px;
  }
</style>
