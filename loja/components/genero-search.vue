<template>
    <app-quote>
        <input type="checkbox" name="genero" :value="name"><label for="">{{name}}</label>
    </app-quote>
</template>
<script>
    import Quote from '@/components/Quote.vue';

    export default {
        components: {
            appQuote: Quote
        },
        props:{
            name:{
                type: String,
                requires: true
            }
        }
    }
</script>

