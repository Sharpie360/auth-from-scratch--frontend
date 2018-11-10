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

        <b-form-group v-show="user.password.length > 5"
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
        <div class="signup-group">
          <b-button type="submit" variant="success"><span v-show="!isRequesting">Sign Me Up!</span>
          <rotate-loader v-show="isRequesting"></rotate-loader>
          </b-button>
        </div>

      </b-form>
    </b-card>
  </section>
</template>

<script>
import Joi from "joi";
import RotateLoader from "../components/RotateLoader";

const SIGNUP_URL_LOCALHOST = "http://localhost:7777/auth/signup";
const SIGNUP_URL_NETWORK = "http://192.168.5.135:7777/auth/signup";

const schema = Joi.object().keys({
  username: Joi.string()
    .regex(/(^[a-zA-Z0-9!#$%^&*_-]+$)/)
    .min(3)
    .max(20)
    .required(),
  password: Joi.string()
    .trim()
    .min(6)
    .required(),
  passwordVerify: Joi.string()
    .trim()
    .min(6)
    .required()
});

export default {
  data() {
    return {
      alertMessage: {
        visible: false,
        isError: false,
        value: ""
      },
      user: {
        username: "",
        password: "",
        passwordVerify: ""
      },
      isRequesting: false
    };
  },
  components: {
    "rotate-loader": RotateLoader
  },
  watch: {
    user: {
      handler() {
        this.alertMessage.visible = false;
        this.alertMessage.value = "";
      },
      deep: true
    }
  },
  methods: {
    signup() {
      this.isRequesting = true;
      if (this.validateUser()) {
        const requestBody = {
          username: this.user.username,
          password: this.user.password
        };

        this.showAlert(
          false,
          "Awesome! Your entries seem to be excellent! Checking avaiability..."
        );
        fetch(SIGNUP_URL_NETWORK, {
          method: "POST",
          body: JSON.stringify(requestBody),
          headers: {
            "content-type": "application/json"
          }
        })
          .then(response => {
            if (response.ok) {
              return response.json();
            }

            return response.json().then(error => {
              this.isRequesting = false;
              throw new Error(error.message);
            });
            // successful signup
          })
          .then(result => {
            localStorage.token = result.token;
            setTimeout(() => {
              this.isRequesting = false;
              this.$router.push("/dashboard");
            }, 1000);
          })
          .catch(error => {
            console.log(error);
            //this.alertMessage.value = error.message
            this.showAlert(true, error.message);
            this.isRequesting = false;
          });
      }
    },
    validateUser() {
      if (this.user.password !== this.user.passwordVerify) {
        this.isRequesting = false;
        this.showAlert(
          true,
          "The passwords you've entered don't seem to match. Please try again..."
        );
        return false;
      }

      const result = Joi.validate(this.user, schema);
      if (result.error === null) {
        return true;
      }

      if (result.error.message.includes("username")) {
        this.isRequesting = false;
        this.showAlert(
          true,
          "The username you claimed is not accepted. Requires a-z, A-Z, 0-9, !#$%^&*_-"
        );
      } else {
        this.isRequesting = false;
        this.showAlert(
          true,
          "The password you've invoked will not work. Requires a-z, A-Z, 0-9, !#$%^&*_-"
        );
        return false;
      }
    },

    showAlert(isError, msg) {
      this.alertMessage.visible = true;
      this.alertMessage.isError = isError;
      this.alertMessage.value = msg;
      const vm = this;
      setTimeout(() => {
        vm.alertMessage.visible = false;
        vm.alertMessage.isError = false;
        vm.alertMessage.value = "";
      }, 3000);
    }
  }
};
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
