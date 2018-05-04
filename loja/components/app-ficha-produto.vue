<template>
    <app-quote>
        <h2>{{produto}}</h2>
        <p>{{valor}}€</p>
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
            }
        }
    }
</script>
