<template>
  <b-container>

    <b-card>
			<h1>{{ welcomeMessage }}</h1>
			<p class="card-text">
				please login to continue
			</p>
			<b-form
				@sumbit.prevent="login">
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
          <b-button type="submit" variant="success"><span v-show="!isRequesting">Login!</span>
          <rotate-loader v-show="isRequesting"></rotate-loader>
          </b-button>
        </div>
			</b-form>
		</b-card>
	</b-container>
</template>

<script>
import RotateLoader from '../components/RotateLoader'

export default {
	data () {
		return {
			welcomeMessage: 'Welcome!',
			user: {
				username: '',
				password: ''
			},
			isRequesting: false
		}
	},
	components: {
		'rotate-loader': RotateLoader
	},
	methods: {
		login() {

		}
	}
}
</script>

<style>

</style>
