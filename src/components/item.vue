<template>
<div class="item-box clearfix">
  <ul class="thing">
      <li v-for="(item,index) in currentArr" :class="{done:item.isDone}">
          <span @dblclick="handleDouble(index)" v-show="!item.isShow">
             {{item.title}}
          </span>
          <input type="text" v-model="item.title" v-show="item.isShow" autofocus @blur="handleBlur(index)">
          <input type="checkbox" v-model="item.isDone" @change="changeStatus(item)">
          <button  @click="removeSelf(item._id)">删除</button>
      </li>
  </ul>
</div>
</template>

<script>
  import axios from 'axios'
    export default {
        name: "item",
        props:{
            arr:{
              type:Array
            },
          getData:{
              type:Function
          }
        },
        data(){
          return{
            currentArr: this.arr,
          }
        },
        watch:{
          arr(val){
            this.currentArr = val;
          },
        },
      methods:{
        changeStatus (item){
          let isDone = item.isDone?1:0;
          let title = item.title;
          let id = item._id;
          axios.patch(`/api/todo/${id}`,{
              isDone,
              title
            }).then(res=>{
              if (res.data.code==200) {
                alert("修改成功!");
                this.getData();
              }
            })
        },
        removeSelf(id){
          axios.delete(`/api/todo/${id}`).then(res=>{
            if (res.data.code==200){
              alert("删除成功了")
              this.getData();
            }
            else {
              alert("删除失败了")
            }
          })
        },
        handleDouble (index){
          this.currentArr[index].isShow=true;
        },
        handleBlur(index){
          this.currentArr[index].isShow=false;
          this.changeStatus(this.currentArr[index]);
        },
      }
    }
</script>

<style scoped>
  .clearfix::after{
    content: "";
    clear: both;
    overflow: hidden;
  }
  .item-box{
    width: 800px;
  }
  .thing{
    /*width: 185px;*/
    height: 50px;
    line-height: 50px;
    color: #000000;
    font-size: 16px;
    /*text-align: center;*/
  }
  .btn1{
    width: 80px;
    height: 50px;
    background: blue;
    color: #fff;
    font-size: 16px;
    text-align: center;
    float: left;
    border: none;
    margin-left: 20px;
    border-radius: 4px;
  }
  ul li.done{
    text-decoration: line-through;
  }
</style>
