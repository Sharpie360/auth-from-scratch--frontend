<template>
  <div class="home">
    <b-jumbotron class="pb-4">
      <!-- <img src="../assets/rabbit-transparent-1000px.png" class="rw-logo"> -->
      <template slot="header">
        Auth From Scratch
      </template>
      <template slot="lead">
        An excellent guide to Authentication and Authorization for beginners!
      </template>
      <hr class="my-4">
      <p>
        Presented by Coding Garden
      </p>
        <router-link class="btn btn-success mt-4 mr-3" :to="{ name: 'signup' }">Sign Up</router-link>
        <router-link class="btn btn-outline-info mt-4" :to="{ name: 'signup' }">Learn More</router-link>
    </b-jumbotron>
  </div>
</template>

<script>

export default {
  name: 'home',
};
</script>

<style>

.jumbotron {
  position: relative;
}

.rw-logo {
  width: 7rem;
  position: absolute;
  right: 1.5rem;
  bottom: 1rem;
}

</style>
