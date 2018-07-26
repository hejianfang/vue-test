<template>
<div>
  <div class="top">
    <div class="wrap">
      <div class="fll">
        ToDoList
      </div>
      <div class="flr">
        <input type="text" placeholder="添加事情" @keyup="up" v-model="title">
      </div>
    </div>
  </div>
</div>
</template>

<script>
  import axios from 'axios'
    export default {
        name: "Header",
      props:{
        getdata:{
          type:Function
        }
      },
      data(){
          return{
          title:'',
          }
      },
      methods:{
          up(event){
            let title = this.title
            if(event.keyCode == 13 && !this.title.trim()){
              alert("请输入要做的事情")
            }
            else if(event.keyCode == 13){
              axios.post("/api/todo",{title}).then(res=>{
                if(res.data.code ==200){
                  this.title = ''
                  this.getdata();
                }
              })
            }
          }
      },
     watch:{

     }
    }
</script>

<style scoped>
.top{
  height: 60px;
  background-color: skyblue;
}
  .wrap{
    width: 30%;
    margin: 0 auto;
    line-height: 60px;
  }
  .fll{
    float: left;
    font-size: 20px;
    font-weight: 700;
    color: #fff;
  }
  .flr{
    float: right;
  }
  .flr input{
    width: 250px;
    height: 30px;
    border-radius: 10px;
    border: none;
    outline: none;
    padding-left: 5px;
  }
</style>
