<template>
    <isAuthenticated>
        <p>ユーザーページ</p>
        <p>こんにちは、{{this.$store.state.user}}</p>
    </isAuthenticated>
</template>

<script>
import isAuthenticated from '~/components/IsAuthenticated.vue'
export default {
    components:{
        isAuthenticated
    }
}
</script>

<style>

</style>
