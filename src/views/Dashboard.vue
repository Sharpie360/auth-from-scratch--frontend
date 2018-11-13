<template>
  <div>
    <h1 class="display-1">Dashboard</h1>
    <h3>{{ user.username }}</h3>

    <b-form>

      <b-form-group
        id="notes--title-group"
        label="Title"
        label-for="notes--title-input">
        <b-form-input
          id="notes--title-input"
          type="text"
          v-model="newNote.title"
          aria-describedby="titleHelp"
          placeholder="New Note Title"
          >
        </b-form-input>
        <small id="titleHelp" class="text-muted">Please enter the title of your note</small>
      </b-form-group>

      <b-form-textarea
        id="notes--body-input"
        v-model="newNote.body"
        placeholder="the body of the note"
        :rows="3"
        :max-rows="8">
      </b-form-textarea>

    </b-form>

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
        
      },
      notes: [],
      newNote: {
        title: '',
        date: '',
        body: '',
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
