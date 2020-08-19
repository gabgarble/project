<template>
  <div>
    <md-dialog :md-active.sync="showDialog">
      <md-dialog-content>
        <md-dialog-title>Add Users</md-dialog-title>

        <md-tabs md-dynamic-height>
          <md-tab md-label="Add new users">
            <h1>Wololo...!!!</h1>
          </md-tab>

          <md-tab md-label="Petitions">
            <h1>Wololo...!!!</h1>
          </md-tab>
        </md-tabs>

        <md-dialog-actions>
          <md-button class="md-primary" @click="showDialog = false">Close</md-button>
          <md-button
            type="submit"
            class="md-primary"
            :disabled="sending"
            @click="showDialog = false"
          >Save</md-button>
        </md-dialog-actions>
      </md-dialog-content>
    </md-dialog>
  </div>
</template>

<!-- Script -->
<script>
export default {
  name: "add-users-form",
  data: () => ({
      sending: false,
      showDialog: false,
  }),
  validations: {

  },
  methods: {
    fShowDialog: function(visible) {
      this.showDialog = visible
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