<template>
    <div class="login">
        <h3></h3>
        <p><input prefix="ios-contact" v-model="username" placeholder="请输入用户名" class="input"/></p>
        <p><input prefix="ios-lock" v-model="password" type="password" placeholder="请输入密码" class="input"/></p>
        <p><checkbox v-model="save">登录名和密码保留一周</checkbox></p>
        <p><button type="primary" class="btn" @click="doLogin()">登录</button>
           <button type="primary" class="btn" to="/register">注册</button></p>
    </div>
</template>

<script>
    import Checkbox from "view-design/src/components/checkbox/checkbox"
    import axios from 'axios'
    export default {
        components: {Checkbox},
        name: "login",
        data(){
            return{
                username:'',
                password:'',
                save:false
            }
        },
        methods:{
            doLogin(){
                axios.post('http://localhost:8080/api/login',{username:this.username,password:this.password})
                    .then((res)=>{
                        if (res.data){
                            if (this.save) {
                                this.$cookies.set('username',this.username,'7d');
                            }
                        this.$store.commit('setLoginUser',this.username);
                        this.$router.push('/productlist');
                    }else{
                    alert('用户名或密码错误，请重新输入');
                    this.username = '';
                    this.password = '';
                    }
                })
            }
        }
    }
</script>

<style scoped>
    .login{width:360px; margin:30px auto; border:1px solid #2d8cf0; padding:20px 30px; border-radius:5px; text-align:left;}
    .login .input{width:280px;}
    .login .btn{width:100px; margin-right:20px;}
    .login p,.login h3{margin-bottom:15px;}
</style>