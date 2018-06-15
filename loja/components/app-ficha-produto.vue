<template>
    <app-quote>
        <h2>{{produto}}</h2>
        <img :src="image" height="120" width="300">
        <p>{{valor}}€ - {{stock}} stock</p>
    </app-quote>
</template>
<script>
    import Quote from '@/components/Quote.vue';

    export default {
        components: {
            appQuote: Quote
        },
        props:{
            produto:{
                type: String,
                requires: true
            },
            valor:{
                type: Number,
                requires: true
            },
            image:{
                type:String,
                requires:true
            },
            stock:{
                type:Number,
                requires: true
            }
        }
    }
</script>

