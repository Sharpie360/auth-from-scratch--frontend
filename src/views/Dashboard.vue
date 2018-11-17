<template>
  <div>
    <h1 class="display-4">Notes</h1>
    <p>Welcome {{ user.username }}! Lets take some notes.</p>
    <hr>
    <h4 
      class="pointer"
      @click="showForm = !showForm"
      >New Note
    </h4>
    <b-alert 
      :show="this.alertMessage.visible"
      :class="{ 
        'alert-msg-danger': this.alertMessage.isError, 
        'alert-msg-success': !this.alertMessage.isError 
      }">
        {{ alertMessage.value }}
    </b-alert>
    <b-form v-if="showForm" @submit.prevent="addNote()">
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
        placeholder="Write out your note here"
        :rows="3"
        :max-rows="8">
      </b-form-textarea>

      <b-button 
        type="submit" 
        class="mt-3"
        variant="success"
        >Add Note
      </b-button>
    </b-form>

    <hr>

    <div class="grid-wrap mb-3">

      <b-card 
        v-for="(note, i) in notes"
        :key="i"
        class="note--card"
        bg-variant="dark" 
        border-variant="success"
        :header="note.title">
        <p class="card-text note--body">{{ note.body }}</p>

      </b-card>
      
    </div>

    <hr>

    <b-button variant="danger" @click="logout">Logout</b-button>

  </div>
</template>

<script>
import { EventBus } from '../event-bus.js'

const API_URL = 'http://localhost:7777'

export default {
  data () {
    return {
      user: {
        
      },
      notes: [],
      newNote: {
        title: '',
        body: '',
      },
      alertMessage: {
        visible: false,
        isError: false,
        value: '',
      },
      showForm: false,
    }
  },
  created() {
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
        this.getNotes()
      } else {
        this.logout()
      }
    })
  },
  methods: {
    getNotes() {
      fetch(`${API_URL}/api/v1/notes`, {
        method: 'GET',
        headers: {
          authorization: `Bearer ${localStorage.token}`
        }
      }).then(res => res.json())
        .then(notes => {
          this.notes.push(...notes)
          console.log(this.notes)
        })
    },
    addNote() {
      fetch(`${API_URL}/api/v1/notes/`, {
        method: 'POST',
        body: JSON.stringify(this.newNote),
        headers: {
          'content-type': 'application/json',
          authorization: `Bearer ${localStorage.token}`
        }
      }).then(res => res.json())
      .then(note => console.log(note))
      this.showAlert(false, 'Your note has been successfully added.')
      this.newNote.title = ''
      this.newNote.body = ''
    },
    logout() {
      localStorage.removeItem('token')
      localStorage.removeItem('afs-userdata')
      EventBus.$emit('isSignedOut')
      this.$router.push('/login')
    },
    showAlert(isError, msg) {
      this.alertMessage.visible = true;
      this.alertMessage.isError = isError;
      this.alertMessage.value = msg;
      const vm = this;
      setTimeout(() => {
        vm.alertMessage.visible = false;
        vm.alertMessage.isError = false;
        vm.alertMessage.value = "";
      }, 3000);
    }

  }
}
</script>

<style>

.grid-wrap {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-gap: 1.25rem
}

.note--card:hover {
  background-color: rgba(3, 169, 172, .2) !important;
}



</style>
