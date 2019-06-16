<template>
  <section class="container text-center">
    <h1 class="h1">Nuxt Bootstrap</h1>
    <div v-if="isAuthenticated">
      {{ user.email }}でログイン中です<br>
      {{ user.displayName }}さん、こんにちは<br>
      <button v-on:click="logout">ログアウト</button><br>
      <a href="/member-page">メンバーページへ</a>
    </div>
    <!-- ログインしていない時に表示される画面 -->
    <div v-else bg-warning text-dark>
      <authPanel />
    </div>
  </section>
</template>
<script>
import firebase from '~/plugins/firebase'
import authPanel from '~/components/AuthPanel.vue'
import 'firebase/auth'
import { mapActions, mapState, mapGetters } from 'vuex'

export default {
  data(){
    return{
      email:'',
      password:'',
      errMsg:[]
    }
  },
  components:{
    authPanel
  },
  methods:{
    ...mapActions(['setUser']),
    login(){
      if(!this.checkForm()){
        return;
      }
      console.log("Login")
      firebase.auth().signInWithEmailAndPassword(this.email, this.password)
      .then(user => {
        // ログインしたら飛ぶページを指定
        // this.$router.push("/member-page")
      }).catch((error) => {
        alert(error)
      });
    },
    checkForm(){
      this.errMsg = [];
      if (!this.email) {
        this.errMsg.push('Email required.');
      }
      if (!this.password) {
        this.errMsg.push('Password required.');
      }

      if(this.errMsg.length==0){
        return true;
      }else{
        return false;
      }
    },
    logout(){
      firebase.auth().signOut()
      .then(() => {
        this.setUser(null)
      }).catch((error) => {
        alert(error)
      })
    }
  },
  computed: {
    ...mapState(['user']),
    ...mapGetters(['isAuthenticated'])
  },
  mounted() {
    firebase.auth().onAuthStateChanged((user) => {
      this.setUser(user)
    })
  },
}
</script>

<style>

</style>

