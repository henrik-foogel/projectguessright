<template>
  <div class="navbar is-centered has-text-white has-background-black">
    <div class="fontawesome-container has-background-primary" v-if="!loggedIn">
      <span class="is-size-5 is-size-6-mobile">Login with:</span>
      <a class="fontawesome is-size-6-mobile" tabindex="0" @keyup.enter="googleLogin" @click="googleLogin">
        <font-awesome-icon :icon="{ prefix: 'fab', iconName: 'google'  }"></font-awesome-icon>
        Google
      </a>
    </div>

    <div class="loggedin ">
      <span v-if="loggedIn">You are signed in as: {{ this.$store.state.currentUser }} </span>
      <span v-if="!loggedIn" class="is-italic"> {{ logoutMessage }} </span>
      <br>
      <button v-if="loggedIn" class="button logout is-primary" tabindex="0" @click="logout">Logout</button>
    </div>
  </div>
</template>

<script>
import firebase from 'firebase'

export default ({
    name: 'Login',
    data() {
      return {
        logoutMessage: ''
      }
    },
    computed: {
      currentUser() {
        return this.$store.state.currentUser
      },
      loggedIn() {
        return this.$store.state.loggedIn 
      }
    },
    methods: {
      // player sign in
      googleLogin(){
      var provider = new firebase.auth.GoogleAuthProvider();
      firebase.auth().signInWithPopup(provider).then(function(result) {
        var user = result.user;
        user.providerData.forEach(function (profile) {
          alert('Welcome, ' + profile.displayName + '!');
          setInterval(function() {
             window.location.href = '/guess'
          }, 1500);
        });
        }).catch(function(error) {
            var errorCode = error.code;
            var errorMessage = error.message;
            alert(errorCode + ": " + errorMessage);
          });
      },
      // player log out
      logout() {
        firebase.auth().signOut().then(() => {
          this.logoutMessage = 'You have signed out from the Guess the Number game!';
          this.$store.state.loggedIn = false;
          this.$store.state.currentUser = null;
        })
      },
    },
});
</script>

<style scoped>
@import '~bulma/css/bulma.css';
  .navbar {
    width: 100%;
  }
  .fontawesome-container {
    padding: 1.5%;
    border: 2px solid white;
    position: fixed;
    top: 0; 
    right: 15px;
  }
  .fontawesome-container > a {
    display: block;
  }
  a {
    color: white;
  }
  .logout {
    margin: 8px 0;
    font-family:  'Black Ops One', cursive;
  }
  .loggedin {
    margin: 1%;
    padding-top: 10px;
    margin: 0 auto;
    font-family:  'Black Ops One', cursive;
  }
  
  @media screen and (max-width: 699px) {
    .fontawesome-container {
      top: 0;
      right: 0;
      left: 0;
      margin: 0;
      padding: 1.5%;
    }
    .fontawesome-container > a {
      display: inline;
    }
    .fontawesome-container > span {
      margin: 2%;
    }
  }
</style>
