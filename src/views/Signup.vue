<template>
  <section>
    <b-card>
      <h1>Sign Up</h1>
      <b-alert 
        :show="this.alertMessage.visible"
        :class="{ 
          'alert-msg-danger': this.alertMessage.isError, 
          'alert-msg-success': !this.alertMessage.isError 
        }">
          {{ alertMessage.value }}
        </b-alert>
      <b-form @submit.prevent="signup">
        <b-form-group 
          id="username-group"
          label="Username:"
          label-for="username-input"
          description="Please claim a unique username">
          <b-form-input 
            id="username-input"
            type="text"
            v-model="user.username"
            placeholder="Enter a Username"
            required>
          </b-form-input>
          <span class="a11y-helper">Enter a username that is longer than 6 characters, and must contain only alpha-numerics and underscores.</span>
        </b-form-group>

        <b-form-group
          id="password-group"
          label="Password:"
          label-for="password-input"
          description="Please choose a password that is at least 6 characters">
          <b-form-input
            id="password-input"
            type="password"
            v-model="user.password"
            placeholder="Make sure it's a strong one!"
            required>
          </b-form-input>
        </b-form-group>

        <b-form-group v-show="user.password.length > 2"
          id="password-verify-group"
          label="Verify Password:"
          label-for="password-verify-input"
          description="Please verify your password">
          <b-form-input
            id="password-verify-input"
            type="password"
            v-model="user.passwordVerify"
            placeholder="Verify that you typed the right thing!">
          </b-form-input>
        </b-form-group>

        <b-button type="submit" variant="success">Sign Me Up!</b-button>

      </b-form>
    </b-card>
  </section>
</template>

<script>
export default {
  data () {
    return {
      alertMessage: {
        visible: false,
        isError: false,
        value: '',
      },
      user: {
        username: '',
        password: '',
        passwordVerify: ''
      }
    }
  },
  methods: {
    signup() {
      if(this.validateUser()) {
        this.showAlert(false, 'Awesome! Your entries seem to be excellent!')
      } else {
        this.showAlert(true, 'Looks like your passwords don\'t match. Please try again...')
      }
    },
    validateUser() {
      if(this.user.password !== this.user.passwordVerify){
        return false
      } else {
        return true
      }
    },
    showAlert(isError, msg) {
      this.alertMessage.visible = true
      this.alertMessage.isError = isError
      this.alertMessage.value = msg
      const vm = this
      setTimeout(() => {
        vm.alertMessage.visible = false
        vm.alertMessage.isError = false
        vm.alertMessage.value = ''
      }, 3000)
    }
  }
}
</script>

<style>

.card {
  background-color: var(--rw-danger-80);
}
.a11y-helper {
  display: none;
}

.alert-msg-danger {
  background-color: rgba(151, 7, 7, 0.75);
  border: 1px solid rgb(194, 8, 8);
  color: var(--rw-font-main);
}
.alert-msg-success {
  background-color: rgba(45, 207, 186, 0.75);
  border: 1px solid rgb(20, 228, 200);
  color: var(--background-grey);
}

</style>
