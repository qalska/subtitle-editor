<template>
  <div class="subtitle-creator-dialog">
    <v-row justify="center">
      <v-dialog
        v-model="dialog"
        persistent
        max-width="600px"
      >
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            color="primary"
            dark
            v-bind="attrs"
            v-on="on"
            class="create"
          >
            Create new subtitle
          </v-btn>
        </template>
        <v-card>
          <v-card-title>
            <span class="headline">Create new subtitle</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col
                  cols="12"
                  sm="6"
                  md="4"
                >
                  <v-text-field
                    label="Start"
                    required
                    type="time"
                    step="1"
                    v-model="startTime"
                  ></v-text-field>
                </v-col>
                <v-col
                  cols="12"
                  sm="6"
                  md="4"
                >
                  <v-text-field
                    label="End"
                    required
                    type="time"
                    step="1"
                    v-model="endTime"
                  ></v-text-field>
                </v-col>
                <v-col
                  cols="12"
                  sm="6"
                  md="4"
                >
                  <v-text-field
                    label="Text"
                    required
                    v-model="text"
                  ></v-text-field>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              color="blue darken-1"
              text
              @click="dialog = false"
            >
              Close
            </v-btn>
            <v-btn
              color="blue darken-1"
              text
              @click="[submitHandler(), dialog = false]"
            >
              Save
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-row>
  </div>
</template>

<script>
export default {
    name: 'SubtitleCreatorDialog',
    data: () => ({
        id: 1,
        startTime: '',
        endTime: '',
        text: '',
        dialog: false,
    }),
    methods: {
        submitHandler() {
            const subtitle = {
                id: this.id++,
                startTime: this.startTime + ',000',
                endTime: this.endTime + ',000',
                text: this.text,
            }
            this.$emit('add-subtitle', subtitle)
        }
    },
}
</script>
<style scoped>
  .subtitle-creator-dialog{
    margin: 20px;
  }
</style>
