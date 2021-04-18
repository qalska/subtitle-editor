<template>
    <div class="row">
        <h2 class="row__h">Create new subtitle</h2>
        <form class="form" @submit.prevent="submitHandler">
            <div class="form__div">
                <label for="start" class="form__label">Start:</label>
                <input v-model="start" type="time" id="start" class="form__input form__input--time" name="start" step="1" required>
            </div>
            <div class="form__div">
                <label for="end" class="form__label">End:</label>
                <input v-model="end" type="time" id="end" class="form__input" name="end" step="1" required>
            </div>
            <div class="form__div">
                <label for="text" class="form__label">Text:</label>
                <textarea v-model="text" name="text" id="text" class="form__input" rows="5" required></textarea>
            </div>
            <button class="form__btn" type="submit">Create subtitle</button>
        </form>
    </div>
</template>

<script>
export default {
    name: 'create',
    data: () => ({
        start: '',
        end: '',
        len: '',
        text: ''
    }),
    methods: {
        submitHandler() {
            const subtitle = {
                id: Date.now(),
                start: this.start,
                end: this.end,
                len: this.len,
                text: this.text
            }
            this.$router.push('/')
        }
    },
}
</script>

<style lang="scss" scoped>
    @mixin flex_center() {
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .row{
        @include flex_center();
        &__h{
            margin: 50px;
        }
    }
    .form{
        @include flex_center();
        &__div{
            margin: 30px 0;
        }
        &__input{
            margin-top: 10px;
            margin-left: 10px;
            border: 1px black solid;
            border-radius: 5px;
            width: 300px;
        }
        &__btn{
            border: 1px black solid;
            border-radius: 5px;
            padding: 5px;
        }
    }

</style>