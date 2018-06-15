<template>
    <div class="container">
        <form @submit.prevent="submeterFormulario" class="form">
            <div class="input-control">
                <label for="" class="erro" v-if="erroadd!=''">{{erroadd}}</label>
                <label>Adicionar Novo Produto</label>
                <hr>
                <br>   
                <label for="">Produto</label>
                <input type="text" v-model="novoProduto.produto">
            </div>
            <div class="input-control">
                <label for="">preço</label>
                <input type="number" v-model="novoProduto.valor">
            </div>
            <div class="input-control">
                <label for="">imagem</label>
                <input type="text" v-model="novoProduto.imgURL">
            </div>
            <br>
            <button type="submit" class="button">enviar</button>
        </form>

        <form @submit.prevent="submeterChaves" class="form">
            <div class="input-control">
                <label for="" class="erro" v-if="errokey!=''">{{errokey}}</label>
                <label for="">Adicionar Chaves A Um Produto Existente</label>
                <hr>
                <br>
                <label for="">Produto</label>
                <input type="text" v-model="newkeys.produto">
            </div>
            <div class="input-control">
                <label>numero de chaves</label>
                <input type="number" v-model="newkeys.chave">
            </div>
            <br>
            <button type="submit" class="button">enviar</button>
        </form>
        
        <form @submit.prevent="eliminarProduto" class="form">
            <div class="input-control">
                <label for="" class="erro" v-if="errodel!=''">{{errodel}}</label>
                <label>Eliminar um produto e todas as suas chaves</label>
                <hr>
                <br>
                <label for="">Produto</label>
                <input type="text" v-model="delProduto.produto">
            </div>
            <br>
            <button type="submit" class="button">enviar</button>
        </form>
    </div>
</template>

<script>
import axios from "axios";
export default {
    data(){
        return{
            novoProduto:{
                produto:"",
                valor:0,
                imgURL:""
            },
            newkeys:{
                produto:"",
                chave:0
            },
            delProduto:{
                produto:"",
                test:0
            },
            errodel:"",
            erroadd:"",
            errokey:"",
        }
    },
    methods:{
        submeterFormulario(){
            var as=this;
            return axios.post("http://localhost:8081/add",this.novoProduto)
            .then(function(res){
                if(res.data=="produto ja existe"){
                    as.erroadd="produto ja existe";
                }else if(res.data=="record added"){
                    window.location.href = '/admin';
                }
            });
        },
        submeterChaves(){
            var as=this;
            return axios.post("http://localhost:8081/addkeys",this.newkeys)
            .then(function(res){
                if(res.data=="produto nao existe"){
                    as.errokey="produto nao existe";
                }else if(res.data=="record added"){
                    window.location.href = '/admin';
                }
            });
        },
        eliminarProduto(){
            var as=this;
            return axios.post("http://localhost:8081/delProduto",this.delProduto)
            .then(function(res){
                if(res.data=="produto nao existe"){
                    as.errodel="produto nao existe";
                }else if(res.data=="record deleted"){
                    window.location.href = '/admin';
                }
            });
        },
        check(){
            if(sessionStorage.getItem(name)==null){
                window.location.href = '/login';
            }
        }
    },
    created(){
        this.check()
    },
}
</script>

<style scoped>
    .container{
        display: flex;
        justify-content: center;
        flex-direction: column;
        align-items: center;
        flex-wrap:wrap;
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
    .erro{
        float: none; 
        background-color: red;
        color: black; 
        vertical-align: bottom;
        padding: 5px;
        border-radius: 10px;
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
