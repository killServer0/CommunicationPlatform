<template>
  <div>
  <a-layout style="min-height:100%">
      <a-layout-content>
        <a-card style="width:300px;margin:100px auto;">
          <h2>登录</h2>
          <a-input placeholder="用户名" ref="usernameInput" v-model="username" style="margin-top:20px">
            <a-icon slot="prefix" type="user" />
          </a-input>
          <a-input-password placeholder="密码" ref="passwordInput" v-model="password" style="margin-top:20px">
            <a-icon slot="prefix" type="info-circle" />
          </a-input-password>
          <a-row type="flex" justify="center" style="margin-top:40px;margin-bottom:30px">
          <a-col :span="11" style="margin-right:10px">
          <a-button type="primary" block @click="toRegister">注册</a-button>
          </a-col>
          <a-col :span="11">
          <a-button type="primary" block @click="checklogin">登录</a-button>
          </a-col>
          </a-row>
        </a-card>
      </a-layout-content>
  </a-layout>
  </div>
</template>

<style>

</style>

<script>
  import Vue from 'vue'
  import global_ from '../components/Global'
  export default{
    data() {
      return {
        username:'',
        password:"",
        token:'',
      };
    },
    created: function(){
      document.title = this.$route.meta.title || this.$route.meta.pathName
    },
    watch:{
      $route(){
        document.title = this.$route.meta.title || this.$route.meta.pathName
      },   
    },
    methods:{
      toRegister(){
      this.$router.push({path:"/register"});
      },
      toIndex(){
        if(global_.token!=''){
          global_.username=this.username;
          console.log(global_.token+this.username)
          this.$router.push({path:"/"});
        }
      },
      checklogin(){
        Vue.axios.post('http://182.92.57.178:5000/login',{
            name:this.username,
            password:this.password
        }).then(function(res){
          global_.token=res.data.result.token;
        }).catch(function(error){
          console.log(error); 
        })
        console.log(global_.token!='')
        this.toIndex()
      }
    }
  }
</script>