<template>
  <b-container>

    <b-card>
			<h1>{{ welcomeMessage }}</h1>
			
			<b-alert 
        :show="this.alertMessage.visible"
        :class="{ 
          'alert-msg-danger': this.alertMessage.isError, 
          'alert-msg-success': !this.alertMessage.isError 
        }">
          {{ alertMessage.value }}
      </b-alert>
			<b-form
				@submit.prevent="login">
				<b-form-group
					id="username-group"
					label="Username:"
					label-for="username-input"
					description="please enter your username">
					<b-form-input
						id="username-input"
						type="text"
						v-model="user.username"
						required>
					</b-form-input>
				</b-form-group>

				<b-form-group
					id="password-group"
					label="Password:"
					label-for="password-input"
					description="please enter your password">
					<b-form-input 
						id="password-input"
						type="password"
						v-model="user.password"
						required>
					</b-form-input>	
				</b-form-group>

				<div class="submit-group">
          <b-button type="submit" variant="success"><span v-show="!isRequesting">Login</span>
          <rotate-loader v-show="isRequesting"></rotate-loader>
          </b-button>
					<b-button variant="outline-info" class="ml-3">
						Sign Up
					</b-button>
        </div>
			</b-form>
		</b-card>
	</b-container>
</template>

<script>
import Joi from 'joi'

import RotateLoader from '../components/RotateLoader'

const schema = Joi.object().keys({
  username: Joi.string().regex(/(^[a-zA-Z0-9!#$%^&*_-]+$)/).min(3).max(20).required(),
  password: Joi.string().trim().min(6).required(),
})

const LOGIN_ROUTE = 'http://localhost/7777/auth/login'

export default {
	data () {
		return {
			welcomeMessage: 'Welcome!',
			user: {
				username: '',
				password: ''
			},
			isRequesting: false,
			alertMessage: {
        visible: false,
        isError: false,
        value: '',
      }
		}
	},
	components: {
		'rotate-loader': RotateLoader
	},
	watch: {
		// user: {
		// 	handler(){
		// 		this.alertMessage.visible = false
		// 		this.alertMessage.value = ''
		// 	},
		// 	deep: true
		// }
	},
	methods: {
		login() {
			this.isRequesting = true
			if(this.validUser()){
				fetch('/auth')
			}
		},
		validUser(){
			const result = Joi.validate(this.user, schema)
			if (result.error === null){
				return true
			}
			if(result.error.message.includes('username')){
				this.showAlert(true, 'Username is outside the spec.')
				
			} else {
				this.showAlert(true, 'Password is outside the spec.')
			}
			this.isRequesting = false
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

</style>
