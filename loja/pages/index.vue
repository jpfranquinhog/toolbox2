<template>
    <div>
        <div class="search-wrapper">
            <input type="text" v-model="search" placeholder="Search title.."/>
        </div>
        <div class="app">
            
            <div class="listaProdutos">
                <app-ficha-produto  class="teste" v-for="(artigo) in artigosFiltrados" :key="artigo.id" :produto="artigo.name" :valor="+artigo.price" :stock="artigo.stock" :image="artigo.image" v-on:click.native="carregaCarrinho(artigo)" style="cursor:pointer"/>
            </div>
            <div class="listaCarrinhoCompras">
                <br>
                <h2>Carrinho de Compras</h2>
                {{ultimaCompra | date}} - {{ sum() }}€
                
                <form @submit.prevent="checkout" class="form">
                    <br>
                    <button type="submit" class="button">checkout</button>
                    <br>
                </form>
                <app-ficha-produto class="teste" v-for="(artigo, index) in carrinhoCompras" :key="artigo.id" :produto="artigo.name" :valor="+artigo.price" :stock="artigo.stock" :image="artigo.image" v-on:click.native="descarregaCarrinho(index)" style="cursor:pointer"/>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from "axios";
    import appFichaProduto from '@/components/app-ficha-produto';

    export default {
        components: {
            appFichaProduto
        },
        data(){
            return{
                carrinhoCompras:[],
                ultimaCompra: false,
                search:"",
                artigos:[]
            }
        },
        methods:{
            carregaCarrinho(artigo){
                if(artigo.stock>0){
                    artigo.stock--;
                    this.carrinhoCompras.unshift({...artigo, dataCompra: new Date()});
                    this.ultimaCompra=this.carrinhoCompras[0].dataCompra;
                }
            },
            descarregaCarrinho(index){
                this.carrinhoCompras.splice(index,1);
            },
            sum(){
                if(this.carrinhoCompras.length < 1){
                    return 0;
                }else{
                    return this.carrinhoCompras.map( (a) => Math.floor(a.price)).reduce((a,b) =>{return a+b})
                }
            },
            checkout(){
                var vm=this;
                return axios.post("http://localhost:8081/checkout/"+sessionStorage.getItem(name),this.carrinhoCompras)
                .then(function(){
                    vm.carrinhoCompras=[];
                    window.location.href = '/';
                });
            },
            check(){
                if(sessionStorage.getItem(name)==null){
                    window.location.href = '/login';
                }
            },
        },            
        computed:{
            artigosFiltrados(){
                var vm=this;
                return this.artigos.filter((cust)=>{return cust.name.toLowerCase().indexOf(vm.search.toLowerCase())>=0;});
            }
        },
        async asyncData() {
            //return axios.get("https://umaprodutos.firebaseio.com/.json")
            return axios.get("http://localhost:8081/")
            .then((res)=>{
                return{artigos:res.data}
            })
        },
        created(){
            this.check()
        },
    }
</script>

<style scoped>
    .app{
        margin-top:1rem;
        display: flex;
        flex-direction: row;
        justify-content: space-around;
    }
    .listaProdutos{
        position: relative;
        display: flex;
        align-items: flex-start;
        justify-content: space-between;
        flex-wrap: wrap;
        z-index: 10;
    }
    .listaCarrinhoCompras{
        display: flex;
        min-width: 500px;
        align-items: center;
        flex-direction: column;
        border-radius: 10px;
        border: 1px solid #06c4d1;
        background-color: #eee;
    }
    .teste{
        margin:1rem;
        border-radius: 20px;
    }
</style>



