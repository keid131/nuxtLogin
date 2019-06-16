<template>
    <div class="authPanelArea">
        <b-tabs content-class="mt-3" align="center">
            <b-tab title="ログイン" active><signinPanel class="signinArea" /></b-tab>
            <b-tab title="アカウント登録"><signupPanel class="signupArea" /></b-tab>
        </b-tabs>   
    </div>
</template>

<script>
import signinPanel from '~/components/SigninPanel.vue'
import signupPanel from '~/components/SignupPanel.vue'
export default {
    components:{
        signinPanel,
        signupPanel
    }
}
</script>

<style>

</style>
