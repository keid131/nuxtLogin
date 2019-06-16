<template>
    <section class="ml-auto mr-auto">
        <div>
            <p>メール</p>
            <input type="email" v-model="email"><br>
        </div>
        <div>
            <p>パスワード</p>
            <input type="password" v-model="password"><br>
        </div>
        <primaryBtn text="ログイン" @push="login" />
        <div v-for="(item, index) in errMsg" v-bind:key="index">
        {{item}}
        </div>
  </section>
</template>

<script>
import firebase from '~/plugins/firebase'
import 'firebase/auth'
import { mapActions, mapState, mapGetters } from 'vuex'
import primaryBtn from '~/components/Button/primaryBtn.vue'
export default {
  data(){
    return{
      email:'',
      password:'',
      errMsg:[]
    }
  },
  components:{
      primaryBtn
  },
  methods:{
    ...mapActions(['setUser']),
    login(){
      if(!this.checkForm()){
        return;
      }

      firebase.auth().signInWithEmailAndPassword(this.email, this.password)
      .then(user => {
        // ログインしたら飛ぶページを指定
        // this.$router.push("/member-page")
        console.log("Login")
        this.setUser(user)
      }).catch((error) => {
        alert(error)
      });
    },
    checkForm(){
      this.errMsg = [];
      if (!this.email) {
        this.errMsg.push('メールアドレスの入力は必須です。');
      }
      if (!this.password) {
        this.errMsg.push('パスワードの入力は必須です。');
      }
      if(this.password.length<8){
          this.errMsg.push('パスワードは8文字以上入力してください');
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
    ...mapState(['user'])
  }
}
</script>

<style>

</style>
