<template>
    <div class="main">
        <form @submit.prevent="login" class="form">
            <div>
                <label for="" class="erro" v-if="erro!=''">{{erro}}</label>
            </div>
            <div class="input-control">
                <label>login</label>
                <hr>
                <br>   
                <label for="">Nome</label>
                <input type="text" v-model="user.username">
            </div>
            <div class="input-control">
                <label for="">password</label>
                <input type="password" v-model="user.password">
            </div>
            <br>
            <button type="submit" class="button">entrar</button>
            <br>
            <label for="">Se ainda nao tem conta va para </label><nuxt-link to="/signup" class="nav-link"><a>signup</a></nuxt-link>
        </form>
    </div>
</template>

<script>
    import axios from "axios";
    export default {
        data(){
            return{
                user:{
                    username:"",
                    password:""
                },
                erro:""
            }
        },
        methods:{
            login(){
                var as=this;
                return axios.post("http://localhost:8081/login",this.user)
                .then(function(res){
                    if(res.data=="user nao encontrado"){
                        //user nao existe
                        as.erro="user nao encontrado";
                    }else if(res.data=="pass errada"){
                        //pass errada
                        as.erro="pass errada";
                    }else if(res.data.id>0){
                        sessionStorage.setItem(name,res.data.username);
                        window.location.href = '/';
                    }
                });
            }
        }
    }
</script>

<style scoped>
    .main{
        display: flex;
        flex-direction: row;
        justify-content: center;
    }
    .erro{
        float: none; 
        background-color: red;
        color: black; 
        vertical-align: bottom;
        padding: 5px;
        border-radius: 10px;
    }
    .form{
        max-width: 550px;
        box-sizing: border-box;
        margin: 30px;
        margin-top: 60px;
        padding: 30px;
        text-align: justify;
        box-shadow: 0 0 24px rgba(0, 0, 0, 0.3);
        width: 100%;
    }
    .button{
        font: inherit;
        cursor: pointer;
        border-radius: 4px;
        border: 1px solid #06c4d1;
        background-color: white;
        color: #06c4d1;
        text-decoration: none;
        padding: 10px 30px;
    }
    .button:hover, .button:active{
        color: #fff;
        background-color: #06c4d1;
    }
    .input-control{
        margin: 10px 0;
    }
    .input-control label{
        display: block;
        font-weight: bold;
        color: #06c4d1;
    }
    .input-control input{
        display: block;
        width: 100%;
        box-sizing: border-box;
        font: inherit;
        border: 1px solid #ccc;
        border-radius: 4px;
        padding: 5px;
    }
    .input-control input:focus{
        background-color: #eee;
        outline:none;
    }
</style>
