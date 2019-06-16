<template>
    <div class="mt-3"> 
       <b-button variant="primary" @click="push" >{{text}}</b-button>
    </div>
</template>

<script>
export default {
    props:{
        text :  String
    },
    methods: {
        push(){
            this.$emit('push');
        }
    }
}
</script>

<style>

</style>
