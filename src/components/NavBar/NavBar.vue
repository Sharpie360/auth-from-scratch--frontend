<template>
  <b-navbar toggleable="md" type="dark" class="rw-navbar">
    <b-navbar-toggle target="nav_collapse"></b-navbar-toggle>

      <b-navbar-brand>
        <router-link id="nav-brand" to="/">Auth From Scratch</router-link>
      </b-navbar-brand>

      <b-collapse is-nav id="nav_collapse">
        <b-navbar-nav class="ml-auto">

          <b-nav-item v-show="!currentUser.signedIn">
            <router-link id="nav-signup" to="/signup">Sign Up</router-link>
          </b-nav-item>

          <b-nav-item v-show="!currentUser.signedIn">
            <router-link to="/login">Login</router-link>
          </b-nav-item>

          <b-nav-item v-show="currentUser.signedIn">
            <router-link to="/dashboard">{{ currentUser.username }}</router-link>
          </b-nav-item>

        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
</template>

<script>
export default {
  data () {
    return {

    }
  },
  props: ['currentUser'],
  methods: {
    
  },
  created() {

  }
}
</script>

<style>

</style>
