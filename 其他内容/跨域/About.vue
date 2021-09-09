<template>
  <div class="about">
    <p>This is an about page</p>
    <h1>{{msg}}</h1>
  </div>
</template>
<script>
export default {
  data() {
    return {
      msg: "default text"
    };
  },
  methods:{
    fetchData(){
      const server01 = "http://127.0.0.1:9000"; 
      const server02 = "http://127.0.0.1:8866"; //nginx代理
      this.$axios.get(server01).then(res=>{
        this.msg = res.data
        console.log(res)
      })
    }
  },
  created(){
    this.fetchData()
  }
};
</script>
