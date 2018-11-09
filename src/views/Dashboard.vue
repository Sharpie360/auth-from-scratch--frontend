<template>
  <div>
    <h1 class="display-1">Dashboard</h1>
    <h3>{{ user.username }}</h3>
    <h6>it werks!</h6>
    <hr>
    <b-button variant="danger" @click="logout">Logout</b-button>
  </div>
</template>

<script>

const API_URL = 'http://localhost:7777'

export default {
  data () {
    return {
      user: {
        
      }
    }
  },

  mounted() {
    fetch(API_URL, {
      headers: {
        authorization: `Bearer ${localStorage.token}`
      }
    }).then(res => res.json())
    .then(result => {
      if (result.user){
        this.user = result.user
      } else {
        this.logout()
      }
    })
  },
  methods: {
    logout() {
      localStorage.removeItem('token')
      this.$router.push('/login')
    }
  }
}
</script>

<style>

</style>
