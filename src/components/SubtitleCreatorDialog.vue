<template>
  <div class="subtitle-creator-dialog">
    <div class="top">
      <button 
      class="create btn btn-primary" 
      type="submit"
      v-on:click="showForm">Create new subtitle</button>
      <div class="field">
        <div class="field__wrapper">
            <input name="file" type="file" id="field__file-2" class="field field__file">         
            <label class="field__file-wrapper" for="field__file-2">
              <div class="field__file-fake">Upload subtitles</div>
              <div class="field__file-button">Browse</div>
            </label>
        </div>
      </div>
    </div>
    <div class="row" v-show="isShow">
      <hr>
      <form class="row__form" @submit.prevent="submitHandler">
        <div class="row__form-row">
          <div class="form-group">
            <label for="start-time">Start</label>
            <input 
            type="time" 
            class="form-control" 
            id="start-time"
            v-model="startTime" 
            step="1" 
            required>
          </div>
          <div class="form-group">
            <label for="end-time">End</label>
            <input type="time" class="form-control" id="end-time" v-model="endTime" step="1" required>
          </div>
          <div class="form-group">
            <label for="text">Text</label>
            <input type="text" class="form-control" id="text" v-model="text" step="1" required>
          </div>
        </div>
        <div class="row__form-btn">
          <button type="button" class="btn btn-outline-danger" v-on:click="hideForm">Close</button>
          <button type="submit" class="btn btn-outline-success" v-on:click="hideForm">Save</button>
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
                startTime: this.startTime + ',000',
                endTime: this.endTime + ',000',
                text: this.text,
            }
            this.$emit('add-subtitle', subtitle)
            this.startTime = ''
            this.endTime = ''
            this.text = ''
        },
        showForm() {
            this.isShow = true;
        },
        hideForm() {
            this.isShow = false;
        },
    },
}
</script>
<style lang="scss" scoped>
  @import '@/scss/file-input.scss';
  @import '@/scss/mixins.scss';
  .subtitle-creator-dialog{
    margin: 20px;
  }
  .top{
    @include flex_space-between();
  }
  .btn{
    &-primary{
      color: white;
    }
    &-outline{
      &-danger{
        color: red;
        margin-right: 20px;
        &:hover{
          color: white;
        }
      }
      &-success{
        color: green;
        &:hover{
          color: white;
        }
      }
    }
  }
  .row{
    &__form{
      &-row{
        margin: 20px 0;
        @include flex_space-between();
      }
      &-btn{
        display: flex;
        flex-direction: row;
        justify-content: flex-end;
      }
    }
  }
</style>
