<template>
  <div>
    <md-dialog :md-active.sync="showDialog">
      <md-dialog-content>
        <md-dialog-title>Import Calendar</md-dialog-title>
        <p></p>
        <form
          id="cimportCalendar"
          novalidate
          class="md-layout"
          @submit.prevent="validateUser"
          action
          method="post"
        >
          <md-card class="md-layout-item md-size-50 md-small-size-100 fix-form-to-screen">
            <md-card-content>
              <md-field>
                <label>Calendar name</label>
                <md-input v-model="regular" md-counter="15"></md-input>
              </md-field>

              <md-field>
                <label>Select a calendar</label>
                <md-file v-model="single" />
              </md-field>

              <md-list>
                <md-list-item @click="showDialog = false">
                  Calendar imported 1
                  <md-icon>delete_outline</md-icon>
                </md-list-item>
                <md-list-item>
                  Calendar imported 2
                  <md-icon>delete_outline</md-icon>
                </md-list-item>
                <md-list-item>
                  Calendar imported 3
                  <md-icon>delete_outline</md-icon>
                </md-list-item>
                <md-list-item>
                  Calendar imported 4
                  <md-icon>delete_outline</md-icon>
                </md-list-item>
              </md-list>
            </md-card-content>
          </md-card>
          <md-dialog-actions>
            <md-button class="md-primary" @click="showDialog = false">Close</md-button>
            <md-button
              type="submit"
              class="md-primary"
              :disabled="sending"
              @click="showDialog = false"
            >Save</md-button>
          </md-dialog-actions>
        </form>
      </md-dialog-content>
    </md-dialog>
  </div>
</template>

<!-- Script -->
<script>
export default {
  name: "import-calendar-form",
  data: () => ({
    sending: false,
    showDialog: false,
    single: null,
    regular: null,
  }),
  validations: {},
  methods: {
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