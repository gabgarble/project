<template>
  <div id="UserRegister">
    <form id="userRegisterForm" novalidate class="md-layout align-to-center-form" @submit.prevent="validateUser"
        action=""
        method="post">
        <md-card class="md-layout-item md-size-50 md-small-size-100">
            <md-card-header>
            <div class="md-title align-to-center-text">Create a new User</div>
            </md-card-header>

            <md-card-content>
              <div class="md-layout md-gutter">
                  <div class="md-layout-item md-small-size-100">
                  <md-field :class="getValidationClass('firstName')">
                      <label for="first-name">First Name</label>
                      <md-input name="first-name" id="first-name" autocomplete="given-name" v-model="form.firstName" :disabled="sending" />
                      <span class="md-error" v-if="!$v.form.firstName.required">The first name is required</span>
                      <span class="md-error" v-else-if="!$v.form.firstName.minlength">Invalid first name</span>
                  </md-field>
                  </div>

                  <div class="md-layout-item md-small-size-100">
                  <md-field :class="getValidationClass('lastName')">
                      <label for="last-name">Last Name</label>
                      <md-input name="last-name" id="last-name" autocomplete="family-name" v-model="form.lastName" :disabled="sending" />
                      <span class="md-error" v-if="!$v.form.lastName.required">The last name is required</span>
                      <span class="md-error" v-else-if="!$v.form.lastName.minlength">Invalid last name</span>
                  </md-field>
                  </div>
              </div>

              <div class="md-layout md-gutter">
                  <div class="md-layout-item md-small-size-100">
                  <md-field :class="getValidationClass('gender')">
                      <label for="gender">Gender</label>
                      <md-select name="gender" id="gender" v-model="form.gender" md-dense :disabled="sending">
                      <md-option></md-option>
                      <md-option value="Male">Male</md-option>
                      <md-option value="Female">Female</md-option>
                      <md-option value="Other">Other</md-option>
                      </md-select>
                      <span class="md-error">The gender is required</span>
                  </md-field>
                  </div>

                  <div class="md-layout-item md-small-size-100">
                      <md-datepicker :class="getValidationClass('birthDate')" v-model="form.birthDate" id="birthDate" name="birthDate">
                          <label>Select date of birth</label>
                          <span class="md-error" v-if="!$v.form.birthDate.required">The birth date is required</span>
                      </md-datepicker>
                  </div>
              </div>

              <div class="md-layout md-gutter">
                  <div class="md-layout-item md-small-size-100">
                  <md-field :class="getValidationClass('typeOfUse')">
                      <label for="typeOfUse">Type of use</label>
                      <md-select name="typeOfUse" id="typeOfUse" v-model="form.typeOfUse" md-dense :disabled="sending">
                      <md-option></md-option>
                      <md-option value="Scholar">Scholar</md-option>
                      <md-option value="Business">Business</md-option>
                      <md-option value="Paticular">Paticular</md-option>
                      </md-select>
                      <span class="md-error">The type of use is required</span>
                  </md-field>
                  </div>
              </div>

              <md-field :class="getValidationClass('email')">
                  <label for="email">Email</label>
                  <md-input type="email" name="email" id="email" autocomplete="email" v-model="form.email" :disabled="sending" />
                  <span class="md-error" v-if="!$v.form.email.required">The email is required</span>
                  <span class="md-error" v-else-if="!$v.form.email.email">Invalid email</span>
              </md-field>

              <div class="md-layout md-gutter">
                  <div class="md-layout-item md-small-size-100">
                    <md-field :class="getValidationClass('password')">
                      <label for="password">Password</label>
                      <md-input v-model="form.password" :disabled="sending" type="password" name="password" id="password"></md-input>
                      <span class="md-error" v-if="!$v.form.password.required">The password is required</span>
                    </md-field>
                  </div>

                  <div class="md-layout-item md-small-size-100">
                    <md-field :class="getValidationClass('repeatPassword')">
                      <label for="repeatPassword">Repeat password</label>
                      <md-input v-model="form.repeatPassword" :disabled="sending" type="password" name="repeatPassword" id="repeatPassword"></md-input>
                      <span class="md-error" v-if="!$v.form.repeatPassword.required">The repeat password is required</span>
                      <span class="md-error" v-if="!$v.form.repeatPassword.sameAsPassword">Passwords must be identical</span>
                    </md-field>
                  </div>
              </div>
            </md-card-content>

            <md-progress-bar md-mode="indeterminate" v-if="sending" />

            <md-card-actions>
                <md-button class="md-raised md-accent" @click="goToLoginPage">Back to login</md-button>
                <md-button type="submit" class="md-raised md-primary" :disabled="sending">Create user</md-button>
                <router-view/>
            </md-card-actions>
        </md-card>
      <md-snackbar :md-active.sync="userSaved">The user {{ lastUser }} was saved with success!</md-snackbar>
    </form>
  </div>
</template>
</template>

<!-- Script -->
<script>
  import { validationMixin } from 'vuelidate'
  import {
    required,
    email,
    minLength,
    maxLength,
    sameAs
  } from 'vuelidate/lib/validators'

  export default {
    name: 'FormValidation',
    mixins: [validationMixin],
    data: () => ({
      form: {
        firstName: null,
        lastName: null,
        gender: null,
        birthDate: null,
        email: null,
        typeOfUse: null,
        password: null
      },
      userSaved: false,
      sending: false,
      lastUser: null
    }),
    validations: {
      form: {
        firstName: {
          required,
          minLength: minLength(3)
        },
        lastName: {
          required,
          minLength: minLength(3)
        },
        birthDate: {
          required
        },
        gender: {
          required
        },
        email: {
          required,
          email
        },
        typeOfUse: {
          required
        },
        password: {
          required,
          minLength: minLength(8)
        },
        repeatPassword: {
          required,
          sameAsPassword: sameAs('password')
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
        this.form.firstName = null
        this.form.lastName = null
        this.form.birthDate = null
        this.form.gender = null
        this.form.email = null
        this.form.typeOfUse = null
        this.form.password = null
        this.form.repeatPassword = null
      },
      saveUser () {
        this.sending = true

        // Instead of this timeout, here you can call your API
        window.setTimeout(() => {
          this.lastUser = `${this.form.firstName} ${this.form.lastName}`
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
      goToLoginPage: function(){this.$router.push('/');}
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
   .md-progress-bar {
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
  }
  .align-to-center-form{
    display: flex;
    justify-content: center;
  }
  .align-to-center-text{
    text-align: center;
  }
  .md-radio {
    display: flex;
  }
</style>