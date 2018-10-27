<template>
  <div class="home">
    <b-jumbotron>
      <template slot="header">
        Auth From Scratch
      </template>
      <template slot="lead">
        An excellent guide to Authentication and Authorization 
      </template>
      <hr class="my-4">
      <p>
        Presented by Coding Garden
      </p>
        <router-link class="btn btn-info" :to="{ name: 'signup' }">Sign Up</router-link>
    </b-jumbotron>
  </div>
</template>

<script>

export default {
  name: 'home',
};
</script>
