<template>
  <div class="grid-wrap mb-3">
    <div class="note-wrapper" v-for="(note, i) in notes" :key="i">
      <dash--note :note="note" :notesArrIndex="i"></dash--note>
    </div>
  </div>
</template>

<script>
import Note from './Note'
export default {
  props: ['notes'],
  components: {
    'dash--note': Note
  }
}
</script>

<style>

</style>
