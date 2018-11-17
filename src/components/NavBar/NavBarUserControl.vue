<template>
  <b-nav-item-dropdown :text="currentUser.username" right>
    <b-dropdown-item class="user-control-item">
      <router-link to="/dashboard">Dashboard</router-link>
    </b-dropdown-item>
    <b-dropdown-item class="user-control-item">
      Notes: {{ currentUser.numberOfNotes }}
    </b-dropdown-item>
    <b-dropdown-item>
      <span @click="triggerLogout">Logout</span>
    </b-dropdown-item>
  </b-nav-item-dropdown>
</template>

<script>
import { EventBus } from '../../event-bus.js'

export default {
  props: ['currentUser'],
  methods: {
    triggerLogout() {
      EventBus.$emit('triggerLogout')
    }
  }
}
</script>

<style>
.navbar-dark .navbar-nav .nav-link {
  color: var(--rw-primary)
}
.navbar-dark .navbar-nav .nav-link:hover {
  color: rgb(255, 255, 255);
}
.dropdown-toggle::after {
  margin-left: .75rem;
  vertical-align: 0.1em;
  border-top: 0.5em solid;
  border-right: 0.5em solid transparent;
  border-left: 0.5em solid transparent
}
</style>
