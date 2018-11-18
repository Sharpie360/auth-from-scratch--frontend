<template>
  <transition name="fade"
    >
    <b-card   
      v-show="note"
      class="note--card"
      bg-variant="dark" 
      border-variant="success"
      :header="note.title">
      <div 
        class="delete-icon pointer" 
        @click="Note_deleteNote(note._id, notesArrIndex)"
        >X
      </div>
      <p 
        class="card-text note--body"
        >{{ note.body }}
      </p>

    </b-card>
  </transition>
</template>

<script>
import { EventBus } from '../../event-bus.js'

export default {
  props: ['note', 'notesArrIndex'],
  methods: {
    
    Note_deleteNote(id, notesArrIndex) {
      EventBus.$emit('deleteNote_trigger', {id, notesArrIndex})
    }
  }
}
</script>

<style>

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s ease-in-out;
}
.fade-enter, .fade-leave-after {
  opacity: 0;
}


</style>
