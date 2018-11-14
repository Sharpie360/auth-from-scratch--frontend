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

          <b-button type="submit" variant="primary"><span v-show="!isRequesting">Login</span>
          <rotate-loader v-show="isRequesting"></rotate-loader>
          </b-button>

					<router-link to="/signup">
						<b-button variant="outline-success" class="ml-3">
							Sign Up
						</b-button>
					</router-link>

        </div>
			</b-form>
		</b-card>
	</b-container>
</template>

<script>
import { EventBus } from '../event-bus.js'
import Joi from 'joi'
import RotateLoader from '../components/RotateLoader'

const schema = Joi.object().keys({
  username: Joi.string().regex(/(^[a-zA-Z0-9!#$%^&*_-]+$)/).min(3).max(20).required(),
  password: Joi.string().trim().min(6).required(),
})

const LOGIN_ROUTE_LOCALHOST = 'http://localhost:7777/auth/login'
const LOGIN_ROUTE_NETWORK = 'http://192.168.5.135:7777/auth/login'

export default {
	name: 'login',
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
	methods: {
		login() {
			this.isRequesting = true
			const requestBody = {
				username: this.user.username,
				password: this.user.password
			}
			if(this.validUser()){
				fetch(LOGIN_ROUTE_NETWORK, {
					method: 'POST',
					headers: {
						'content-type': 'application/json'
					},
					body: JSON.stringify(requestBody)
				}).then(response => {
					if(response.ok){
						return response.json()
					}
					
					return response.json().then(error => {
            throw new Error(error.message)
            this.isRequesting = false
          })
        }).then(result => {
					localStorage.token = result.token
          setTimeout(() => {
						this.isRequesting = false
						EventBus.$emit('isSignedIn', this.user.username)
						this.$router.push('/dashboard')
          }, 1000)
        }).catch(error => {
          console.log(error)
          this.showAlert(true, error.message)
          this.isRequesting = false
				})
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
