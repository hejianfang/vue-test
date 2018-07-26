<template>
    <div>
      <Header :getdata="getdata"></Header>
      <connent :arr="arr" :getdata="getdata"></connent>
    </div>
</template>

<script>
  import Header from '../components/Header'
  import connent from '../components/connent'
  import axios from 'axios'
    export default {
        name: "index",
        components:{Header,connent},
      data(){
          return{
            arr:[]
          }
      },
      methods:{
        getdata(){
          axios.get("/api/todo").then(res=>{
            this.arr = res.data.data.map(item => {
              item.isShow = false;
              return item
            })
          })
        }
      },
      mounted(){
          this.getdata();
      }
    }
</script>

<style scoped>

</style>
