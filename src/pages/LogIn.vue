<template>
  <div class="parent">
    <el-card class="login">
    <h3>Log In</h3>
     <el-input placeholder="Email" v-model="email" clearable>
    </el-input>
    <p></p>
    <el-input placeholder="Password" v-model="password" type="password" clearable>
      </el-input>
    <p></p>
    <el-button type="success" v-on:click="signIn">Log In</el-button>
    <el-button type="success" v-on:click="signInWithGoogle">Google </el-button>
       <span>You don't have an account ? You can
      <router-link to="/signup">create one</router-link></span>
  </el-card>
  </div>
</template>

<script>
  import {Firebase} from "../middleware/firebase";

  const provider = new Firebase.auth.GoogleAuthProvider();

export default {
  name: "login",
  data: function() {
    return {
      email: "",
      password: ""
    };
  },
  methods: {
    signIn() {
      Firebase.auth()
        .signInWithEmailAndPassword(this.email, this.password)
        .then(
          user => {
            this.$router.replace('feeds');
          },
          err => {
            alert("Oops. " + err.message);
          }
        );
    },
    signInWithGoogle() {
      //TODO: Check to send the user to the Feeds page after successful login
      Firebase.auth()
        .signInWithPopup(provider)
        .then(function(result) {
          const token = result.credential.accessToken;
          this.$router.replace("feeds");
          const user = result.user;
        })
        .catch(function(error) {
          alert("Oops. " + error.message);
        });
    }
  }
};
</script>

<style scoped>
/* "scoped" attribute limit the CSS to this component only */

.parent {
  background-image: url(../assets/background-a2f2fdf7dcdaafe1c52909ac3b643dea.svg);
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}

.login {
  width: 500px;
  height: 300px;
  font-family: "Roboto", sans-serif;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;

  margin: auto;
}

span {
  display: block;
  margin-top: 20px;
  font-size: 13px;
}
</style>
