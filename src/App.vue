<template>
  <div id="app">
    <b-navbar toggleable="md" type="dark" class="rw-navbar">
      <b-navbar-toggle target="nav_collapse"></b-navbar-toggle>
      <b-navbar-brand>
        <router-link id="nav-brand" to="/">Auth From Scratch</router-link>
      </b-navbar-brand>
      <b-collapse is-nav id="nav_collapse">
        <b-navbar-nav class="ml-auto">

          <b-nav-item v-show="!checkIfLoggedIn">
            <router-link id="nav-signup" to="/signup">Sign Up</router-link>
          </b-nav-item>

          <b-nav-item v-show="!checkIfLoggedIn">
            <router-link to="/login">Login</router-link>
          </b-nav-item>

          <b-nav-item v-show="checkIfLoggedIn">
            <router-link to="/dashboard">Dashboard</router-link>
          </b-nav-item>

        </b-navbar-nav>
      </b-collapse>
    </b-navbar>

    <router-view class="container pt-4"/>
  </div>
</template>

<script>
import eventBus from './main.js'
export default {
  data() {
    return {
    }
  },
  methods: {
    checkIfLoggedIn() {
      if (localStorage.token) {
        return true;
      } else {
        return false;
      }
    }
  },

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
