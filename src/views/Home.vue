<template>
    <div class="content-row">
        <div class="subtitles">
            <Create 
            class="create"
            @add-subtitle="addSubtitle"
            />
            <hr>
            <table class="subtitles__table table">
                <thead>
                    <tr>
                        <th scope="col">№</th>
                        <th scope="col">Start</th>
                        <th scope="col">End</th>
                        <th scope="col">Text</th>
                    </tr>
                </thead>
                <tbody>
                    <tr
                    v-for="(subtitle, idx) of subtitles"
                    :key="subtitle.id"
                    >
                        <th scope="row">{{idx + 1}}</th>
                        <th>{{subtitle.start}} </th>
                        <th>{{subtitle.end}}</th>
                        <th>{{subtitle.text}}</th>
                    </tr>
                </tbody>
            </table>
        </div>
        <div class="video">
            <div class="input-group">
                <input type="file" class="form-control" id="inputGroupFile02">
                <button class="btn btn-dark">Load</button>
            </div>
            <video controls src="" class="video__tag">
                <track
                default
                kind="captions"
                srclang="en"
                >
            </video>
        </div>
    </div>
</template>

<script>
import Create from '@/components/Create'
export default {
    components: {
        Create
    },
    data() {
        return {
            subtitles: []
        }
    },
    methods: {
        addSubtitle(subtitle) {
            this.subtitles.push(subtitle)
        }
    }
}
</script>

<style lang="scss" scoped>
    .content-row{
        display: flex;
    }
    .subtitles{
        width: 50%;
    }
    .create{
        margin: 20px;
        width: 30%;
    }
    .video{
        width: 50%;
        &__tag{
            margin: 15px 0 0 50px;
            width: 80%;
        }
    }
    .input-group{
        margin: 20px 50px 0px 50px;
        width: 80%;
    }
</style>