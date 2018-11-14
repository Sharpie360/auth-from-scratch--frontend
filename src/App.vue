<template>
  <div id="app">
    <navbar-main :currentUser="currentUser"></navbar-main>
    <router-view class="container pt-4"/>
  </div>
</template>

<script>
import { EventBus } from './event-bus.js'
import NavBar from './components/NavBar/NavBar'

export default {
  data() {
    return {
      currentUser: {
        signedIn: false,
        username: ''
      }
    }
  },
  components: {
    'navbar-main': NavBar
  },
  methods: {
    checkIfLoggedIn() {
      if (localStorage.token) {
        this.currentUser.signedIn = true
      } else {
        this.currentUser.signedIn = false
      }
    }
  },
  created(){
    EventBus.$on('isSignedIn', username => {
      this.currentUser.signedIn = true
      this.currentUser.username = username
    })
    EventBus.$on('isSignedOut', () => {
      this.currentUser.signedIn = false
      this.currentUser.username = ''
    })
  }
};
</script>

<style>
:root {
  --rw-font-main: #f7f7f7;
  --rw-primary: #03a9ac; /* main turquoise */
  --rw-success: #00d6a4; /* bright green   */
  --rw-info: #03d8cb; /* accent teal    */
  --rw-danger: #006473; /* dark turquoise */
  --rw-danger-80: rgba(0, 100, 115, 0.8); /* dark turquoise */
  --rw-background-grey: #212121;
  --rw-off-black: #0a0a0a;
}

body {
  background-color: var(--rw-background-grey);
  color: var(--rw-font-main);
}
hr {
  background-color: rgb(209, 209, 209);
  height: 1px;
}
.rw-navbar {
  background-color: var(--rw-off-black);
}
#nav-brand {
  color: var(--rw-font-main);
}
#nav-signup {
  color: var(--rw-success);
}
.jumbotron {
  background-color: rgba(0, 100, 115, 0.8);
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
