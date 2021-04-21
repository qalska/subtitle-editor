<template>
    <div>
        <Create 
        class="create"
        @add-subtitle="addSubtitle"
        />
        <hr>
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">№</th>
                    <th scope="col">Start</th>
                    <th scope="col">End</th>
                    <th scope="col">Length</th>
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
                    <th>{{subtitle.len}}</th>
                    <th>{{subtitle.text}}</th>
                </tr>
            </tbody>
        </table>
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

<style scoped>
    .create{
        margin: 20px;
        width: 30%;
    }
</style>