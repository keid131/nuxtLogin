<template>
    <section class="ml-auto mr-auto">
        <div>
            <p>ユーザー名</p>
            <input type="text" v-model="displayName"><br>
        </div>
        <div>
            <p>メール</p>
            <input type="email" v-model="email"><br>
        </div>
        <div>
            <p>パスワード</p>
            <input type="password" v-model="password"><br>
        </div>
        <primaryBtn text="アカウント登録" @push="CreateUser" />
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
      displayName: '',
      errMsg:[]
    }
  },
  components:{
      primaryBtn
  },
  methods:{
    ...mapActions(['setUser']),
    CreateUser(){
      if(!this.checkForm()){
        return;
      }

      firebase.auth().createUserWithEmailAndPassword(this.email, this.password)
      .then(user => {
        // ログインしたら飛ぶページを指定
        // this.$router.push("/member-page")
        console.log("CreateUser")
        this.setDisplayName()
        if(this.errMsg.length!=0){
            return;
        }
        this.setUser(user)
        this.$router.push("/test")
      }).catch((error) => {
        this.errMsg.push(error)
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
    setDisplayName(){
        var user = firebase.auth().currentUser;
        user.updateProfile({
            displayName: this.displayName
        }).then(function() {
            // Update successful.
            return
        }).catch(function(error) {
            // An error happened.
            this.errMsg.push(error)
            alert(error)
        });
    },
    logout(){
      firebase.auth().signOut()
      .then(() => {
        this.setUser(null)
      }).catch((error) => {
        this.errMsg.push(error)
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
