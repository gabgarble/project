<template>
  <div>    
    <form id="userLoginForm" novalidate class="md-layout align-to-center-form" @submit.prevent="validateUser"
      action=""
      method="post">
      <md-card class="md-layout-item md-size-30 md-small-size-100">
        <md-card-header >
          <img class="align-to-center" src="./assets/newLogo256.png">
        </md-card-header>
        <md-card-content>
          <md-field :class="getValidationClass('email')">
            <label for="email">Email</label>
            <md-input type="email" name="email" id="email" autocomplete="email" v-model="form.email" :disabled="sending" />
            <span class="md-error" v-if="!$v.form.email.required">The email is required</span>
            <span class="md-error" v-else-if="!$v.form.email.email">Invalid email</span>
          </md-field>
          <md-field :class="getValidationClass('password')">
            <label for="password">Password</label>
            <md-input v-model="form.password" :disabled="sending" type="password" name="password" id="password"></md-input>
            <span class="md-error" v-if="!$v.form.password.required">The password is required</span>
          </md-field>
        </md-card-content>
        <md-card-actions>
          <md-button class="md-raised md-accent" @click="goToUserRegister">Sign in</md-button>
          <!-- <md-button type="submit" class="md-raised md-primary" :disabled="sending">Login</md-button> -->
          <md-button class="md-raised md-primary" @click="goToCalendar">Login</md-button>
          <router-view/>
        </md-card-actions>
      </md-card>
    </form>
  </div>
</template>

<!-- Script -->
<script>
import { validationMixin } from 'vuelidate'
  import {
    required,
    email,
    minLength,
    maxLength
  } from 'vuelidate/lib/validators'

  export default {
    name: 'LoginPage',
    mixins: [validationMixin],
    data: () => ({
      form: {
        email: null,
        password: null
      },
      userSaved: false,
      sending: false,
      lastUser: null
    }),
    validations: {
      form: {
        password: {
          required,
          minLength: minLength(8)
        },
        email: {
          required,
          email
        }
      }
    },
    methods: {
      getValidationClass (fieldName) {
        const field = this.$v.form[fieldName]

        if (field) {
          return {
            'md-invalid': field.$invalid && field.$dirty
          }
        }
      },
      clearForm () {
        this.$v.$reset()
        this.form.email = null
        this.form.password = null
      },
      saveUser () {
        this.sending = true

        // Instead of this timeout, here you can call your API
        window.setTimeout(() => {
          this.lastUser = `${this.form.email}`
          this.userSaved = true
          this.sending = false
          this.clearForm()
        }, 1500)
      },
      validateUser () {
        this.$v.$touch()

        if (!this.$v.$invalid) {
          this.saveUser()
        }
      },
      goToUserRegister: function(){this.$router.push('/UserRegister');},
      goToCalendar: function(){this.$router.push('/Calendar');}
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  small {
    display: block;
  }
  .align-to-center{
    float: none;
  }

.align-to-center-form{
  display: flex;
  justify-content: center;
}
</style>
