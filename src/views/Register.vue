<template>
    <div class="register">
        <p>
            <input prefix="ios-cloudy-outline" placeholder="请输入用户名：" class="input" v-model="username" @on-blur="validateUser()">
            <span class="err">{{errmsg1}}</span>
        </p>
        <p>
            <input prefix="ios-cloudy-outline" placeholder="请输入密码：" class="input" v-model="password" @on-blur="validatePass()">
            <span class="err">{{errmsg2}}</span>
        </p>
        <p>
            <input prefix="ios-cloudy-outline" placeholder="请再次确认密码：" class="input" v-model="passagain" @on-blur="validatePass2()">
            <span class="err">{{errmsg3}}</span>
        </p>
        <button type="primary" class="btn" @click="doRegister()">注册</button>
        <button type="primary" class="btn">重置</button>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        name: "register",
        data(){
            return{
                username:'',
                errmsg1:'',
                errmsg2:'',
                errmsg3:'',
                password:'',
                passagain:''
            }
        },
        methods:{
            validateUser(){
                let valid = true;
                if (this.username == ''){
                    this.errmsg1 = '用户名不能为空';
                    valid = false;
                }else {
                    axios.get('http://localhost:8080/api/isexist',{params:{username:this.username}})
                        .then((res)=>{
                            if (res.data){
                                this.errmsg1 = '用户名已存在';
                                valid = false;
                            }
                        })
                }
                if (valid == true)
                    this.errmsg1 = '';
                return valid;
            },
            validatePass(){
                let valid =true;
                if (this.password == ''){
                    this.errmsg2 = '密码不能为空';
                    valid = false;
                }
                else {
                    var reg = new RegExp(/^\w{6,16}$/)
                    valid = reg.test(this.password);
                    if (!valid)
                        this.errmsg2 = '密码需为6-16位字符';
                }
                if (valid)
                    this.errmsg2 = '';
                return valid;
            },
            validatePass2(){
                let valid =true;
                if (this.passagain == ""){
                    valid = false;
                    this.errmsg3 = '请输入确认密码';
                }
                else {
                    if (this.passagain!=this.password){
                        valid = false;
                        this.errmsg3 = '两次密码不一致';
                    }
                }
                if (valid)
                    this.errmsg3 = '';
                return valid;
            },
            validate(){
                let validate1 = this.validateUser();
                let validate2 = this.validatePass();
                let validate3 = this.validatePass2();
                return validate1&&validate2&&validate3;
            },
            doRegister(){
                axios.post('http://localhost:8080/api/register',{username:this.username,password:this.password})
                    .then((res)=>{
                        if (res.data)
                            alert("注册成功，将跳转到登录页面");
                            this.$router.push('/login')
                    })
            }
        }
    }
</script>

<style scoped>
    .register{width:480px; margin:30px auto; border:1px solid #2d8cf0; padding:20px 5px 10px 30px; border-radius:5px; text-align:left;}
    .err{color:#ed4014;}
    .register .input{width:280px;}
    .register .btn{width:100px; margin-right:20px;}
    .register p,.register h3{margin-bottom:15px;}
</style>