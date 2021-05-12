<template>
  <div class="subtitle-creator-dialog">
    <div class="top">
      <v-btn
        color="accent"
        type="submit"
        @click="isShow = true"
        large
      >Create new subtitle</v-btn>
    </div>
    <div class="row" v-show="isShow">
      <hr>
      <form class="row__form" @submit.prevent="submitHandler">
        <div class="row__form-row">
          <div class="row__form-input">
            <v-text-field
              label="Start"
              type="time"
              id="start-time"
              v-model="startTime" 
              step="1" 
              required
            ></v-text-field>
            <v-text-field
              label="End"
              type="time"
              class="row__form-time"
              id="end-time"
              v-model="endTime" 
              step="1" 
              required
            ></v-text-field>
          </div>
          <v-text-field
            label="Text"
            type="text" 
            id="text"
            v-model="text" 
            step="1" 
            required
          ></v-text-field>
        </div>
        <div class="row__form-btn">
          <v-btn
            text
            color="error"
            @click="isShow = false">
            Close
          </v-btn>
          <v-btn
            text
            color="success"
            type="submit"
            @click="isShow = false">
            Save
          </v-btn>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
    name: 'SubtitleCreatorDialog',
    data() {
      return {
        id: 1,
        startTime: '',
        endTime: '',
        text: '',
        isShow: false,
      }
    },
    methods: {
        submitHandler() {
            const subtitle = {
                id: this.id++,
                startTime: this.startTime + '.000',
                endTime: this.endTime + '.000',
                text: this.text,
                isEditing: false,
            }
            this.$emit('add-subtitle', subtitle)
            this.startTime = ''
            this.endTime = ''
            this.text = ''
        },
    },
}
</script>
<style lang="scss" scoped>
  @import '@/scss/mixins.scss';
  .subtitle-creator-dialog{
    margin: 20px;
  }
  .top{
    margin-bottom: 30px;
  }
  .row{
    &__form-row{
        margin: 20px 0;
    }
    &__form-input{
      @include flex_space-between();
    }
    &__form-time{
      margin-left: 20px;
    }
    &__form-btn{
      display: flex;
      flex-direction: row;
      justify-content: flex-end;
    }
  }
</style>
