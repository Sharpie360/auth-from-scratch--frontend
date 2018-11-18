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
          aria-required="true"
          placeholder="New Note Title"
          required>
        </b-form-input>
        <small id="titleHelp" class="text-muted">Please enter the title of your note</small>
      </b-form-group>

      <b-form-textarea
        id="notes--body-input"
        v-model="newNote.body"
        placeholder="Write out your note here"
        :rows="3"
        :max-rows="8"
        required>
      </b-form-textarea>

      <b-button 
        type="submit" 
        class="mt-3"
        variant="success"
        >Add Note
      </b-button>
    </b-form>

    <hr>

      <dash--noteboard :notes="notes"></dash--noteboard>

    <hr>

  </div>
</template>

<script>
import { EventBus } from '../event-bus.js'

import Noteboard from '../components/Dashboard/Noteboard'

const API_URL = 'http://192.168.5.135:7777'

export default {
  data () {
    return {
      user: {
        _id: '',
        username: '',
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
  components: {
    'dash--noteboard': Noteboard
  },
  created() {
    EventBus.$on('triggerLogout', () => {
      this.logout()
    })
    // Listener for DELETE NOTE from 'Note' cmp
    EventBus.$on('deleteNote_trigger', noteData => {
      console.log(noteData)
      this.notes.splice(noteData.notesArrIndex, 1)
      this.deleteNote(noteData.id)
    })
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
        console.log(this.user)
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
          this.notes = notes
          
          EventBus.$emit('loadNoteCount', this.notes.length)
          console.log(this.notes)
        })
    },
    addNote() {
      fetch(`${API_URL}/api/v1/notes`, {
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
      this.getNotes()
      
    },
    deleteNote(id) {
      const noteID = JSON.stringify({
        "_id": id
      })
      fetch(`${API_URL}/api/v1/notes/rm/`, {
        method: 'POST',
        body: noteID,
        headers: {
          'content-type': 'application/json',
          authorization: `Bearer ${localStorage.token}`
        }
      }).then(res => res.json())
      .then(result => console.log(result))
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

.delete-icon {
  position: absolute;
  top: .75rem;
  right: 1rem;
}

@media screen and (max-width: 480px){
  .grid-wrap {
    display: grid;
    grid-template-columns: 1fr;
    grid-gap: 1.25rem
  }
}

</style>
