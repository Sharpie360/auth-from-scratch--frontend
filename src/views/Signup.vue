<template>
  <section>
    <h1>Sign Up</h1>
    <b-form>

      <b-form-group 
        id="username-group"
        label="Username:"
        label-for="username-input"
        description="Please claim a unique username">
        <b-form-input 
          id="username-input"
          type="text"
          v-model="signup.username"
          placeholder="Enter a Username"
          required>
        </b-form-input>
      </b-form-group>

      <b-form-group
        id="password-group"
        label="Password:"
        label-for="password-input"
        description="Please choose a password that is at least 6 characters">
        <b-form-input
          id="password-input"
          type="password"
          v-model="signup.password"
          placeholder="Make sure it's a strong one!"
          required>
        </b-form-input>
      </b-form-group>

      <b-form-group v-show="signup.password.length > 2"
        id="password-verify-group"
        label="Verify Password:"
        label-for="password-verify-input"
        description="Please verify your password">
        <b-form-input
          id="password-verify-input"
          type="password"
          v-model="signup.passwordVerify"
          placeholder="Verify that you typed the right thing!">
        </b-form-input>
      </b-form-group>

      <b-button variant="info">Sign Me Up!</b-button>

    </b-form>
  </section>
</template>

<script>
export default {
  data () {
    return {
      signup: {
        username: '',
        password: '',
        passwordVerify: ''
      }
    }
  }
}
</script>

<style>

</style>
