<template>
    <div>
      <ul>
        <li v-for="(item,index) in currentArr">
          <input type="checkbox" v-model="item.isDone"
                 @change="changestatus(item._id,item.isDone,item.title)">
          <span :class="{done:item.isDone}" @dblclick="change(index)"  v-show="!item.isShow">
            {{item.title}}
          </span>
          <input type="text" v-show="item.isShow"
                 autofocus
                 @blur="handleBlur(index)"
                 v-model="item.title"
                 @keyup.enter="handlekeyup($event)"
                 class="input" style="width: 200px">
          <button @click="remove(item._id)">删除</button>
        </li>
      </ul>

    </div>
</template>

<script>
  import axios from 'axios'
    export default {
        name: "connent",
      props:{
        arr:{
          type:Array
        },
        getdata:{
          type:Function
        }
      },
      data(){
          return{
            currentArr: this.arr,
          }
      },
      methods:{
        changestatus(id,value,titl){
          let isDone = value?1:0;
          let title = titl
          axios.patch(`/api/todo/${id}`,{
            isDone,title
          }).then(res=>{
              if(res.data.code==200){
                this.getdata();
              }
            })
        },
        remove(id){
          axios.delete(`/api/todo/${id}`).then(res=>{
            if(res.data.code==200){
              this.getdata();
            }
          })
        },
        change(index){
          this.currentArr[index].isShow = true
        },
        handleBlur (index) {
          this.currentArr[index].isShow = false;
          this.changestatus();
        },
        handlekeyup($event){
          $event.target.blur();
        }
      },
      watch: {
          arr (val) {
            this.currentArr = val;
          }
      }
    }
</script>

<style scoped>
ul li{
  width: 30%;
  margin: 20px auto;
background-color: burlywood;
  padding: 20px;
  border-radius: 5px;
  border-left: 5px solid #629A9C;
  box-shadow: 0 1px 2px rgba(0,0,0,0.07);
}
  li button{
    float: right;
    padding: 3px;
    border-radius: 5px;
    outline: none;
    vertical-align: top;
    cursor: pointer;
  }
  li input{
    width: 20px;
    height: 20px;
    vertical-align: top;
    cursor: pointer;
  }
  li span{
    cursor: pointer;
  }
  .done{
    text-decoration: line-through;
    color: green;
  }
</style>
