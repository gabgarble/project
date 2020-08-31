<template>
  <div>
    <md-dialog :md-active.sync="showDialog">
      <md-dialog-content>
        <md-dialog-title>Add New Event</md-dialog-title>
        <p></p>
        <form
          id="createEvent"
          novalidate
          class="md-layout align-to-center fix-form-to-screen"
          @submit.prevent="validateUser"
          action
          method="post"
        >
          <md-card class="md-layout-item md-size-50 md-small-size-100 fix-form-to-screen">
            <md-card-content>
              <div class="md-layout md-gutter">
                <div class="md-layout-item md-small-size-100">
                  <md-field :class="getValidationClass('eventTitle')">
                    <label for="event-title">Event title...</label>
                    <md-input
                      name="event-title"
                      id="event-title"
                      autocomplete="given-name"
                      v-model="form.eventTitle"
                      :disabled="sending"
                    />
                    <span
                      class="md-error"
                      v-if="!$v.form.eventTitle.required"
                    >The first name is required</span>
                    <span
                      class="md-error"
                      v-else-if="!$v.form.eventTitle.minlength"
                    >Invalid event title name</span>
                  </md-field>
                </div>
              </div>

              <div class="md-layout md-gutter">
                <div class="md-layout-item md-small-size-100">
                  <md-field>
                    <label for="textarea">Description...</label>
                    <md-textarea name="textarea" v-model="form.textarea"></md-textarea>
                  </md-field>
                </div>
              </div>

              <div class="md-layout md-gutter">
                <div class="md-layout-item md-small-size-100">
                  <md-datepicker
                    :class="getValidationClass('date')"
                    v-model="form.date"
                    id="date"
                    name="date"
                  >
                    <label>Select date of event</label>
                    <span class="md-error" v-if="!$v.form.date.required">The date is required</span>
                  </md-datepicker>
                </div>
              </div>

              <div class="inline">
                <md-radio
                  :class="getValidationClass('radio')"
                  v-model="form.radio"
                  value="weekly"
                >Weekly</md-radio>
                <md-radio
                  :class="getValidationClass('radio')"
                  v-model="form.radio"
                  value="monthly"
                >Monthly</md-radio>
                <md-radio
                  :class="getValidationClass('radio')"
                  v-model="form.radio"
                  value="anually"
                >Annually</md-radio>
                <md-radio
                  :class="getValidationClass('radio')"
                  v-model="form.radio"
                  value="daily"
                >Daily</md-radio>
                <md-radio
                  :class="getValidationClass('radio')"
                  v-model="form.radio"
                  value="none"
                >None</md-radio>
              </div>
            </md-card-content>
          </md-card>
        </form>

        <md-dialog-actions>
          <md-button class="md-primary" @click="showDialog = false">Close</md-button>
          <md-button
            type="submit"
            class="md-primary"
            :disabled="sending"
          >Save</md-button>
        </md-dialog-actions>
        <md-snackbar :md-active.sync="userSaved">The user {{ lastUser }} was saved with success!</md-snackbar>
        
      </md-dialog-content>
    </md-dialog>
  </div>
</template>

<!-- Script -->
<script>
import { validationMixin } from "vuelidate";
import { required, minLength, maxLength } from "vuelidate/lib/validators";

export default {
  name: "create-event-form",
  mixins: [validationMixin],
  data: () => ({
    form: {
      eventTitle: null,
      textarea: null,
      date: null,
      radio: "none",
    },
    userSaved: false,
    sending: false,
    lastUser: null,
    showDialog: false,
  }),
  validations: {
    form: {
      eventTitle: {
        required,
        minLength: minLength(3),
      },
      date: {
        required,
      },
    },
  },
  methods: {
    getValidationClass(fieldName) {
      const field = this.$v.form[fieldName];

      if (field) {
        return {
          "md-invalid": field.$invalid && field.$dirty,
        };
      }
    },
    clearForm() {
      this.$v.$reset();
      this.form.eventTitle = null;
    },
    saveUser() {
      this.sending = true;

      // Instead of this timeout, here you can call your API
      window.setTimeout(() => {
        //this.lastUser = `${this.form.firstName} ${this.form.lastName}`
        this.userSaved = true;
        this.sending = false;
        this.clearForm();
      }, 1500);
    },
    validateUser() {
      this.$v.$touch();

      if (!this.$v.$invalid) {
        this.saveUser();
      }
    },
    fShowDialog: function (visible) {
      this.showDialog = visible;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.md-progress-bar {
  position: absolute;
}
.align-to-center {
  display: flex;
  justify-content: center;
  border-bottom: 5px;
}

.fix-form-to-screen {
  min-width: 100%;
  min-height: 100%;
}

.changing-inputs-size {
  width: 300px;
}

.md-radio {
  display: flex;
}

.inline {
  display: inline-block;
  width: 50px;
  height: 20px;
}
</style>