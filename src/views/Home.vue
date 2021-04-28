<template>
    <div class="content-row">
        <div class="subtitles">
            <div class="subtitles__top">
                <Create 
                class="create"
                @add-subtitle="addSubtitle"
                />
                <v-file-input
                    accept=".vtt"
                    label="Upload subtitles"
                    class="file-input file-input__sub"
                ></v-file-input>
            </div>
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
            <div class="video__input">
                <input type="file" id="fileElem" accept="video/*" ref="myFiles" @change="onFileSelected()">
                <div class="video__wrapper" id="fileWrapper">
                </div>
            </div>
            <!-- <video controls src="" class="video__tag" id="myvideo">
                <track
                default
                kind="captions"
                srclang="en"
                >
            </video> -->
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
            subtitles: [],
            files: [],
        }
    },
    methods: {
        addSubtitle(subtitle) {
            this.subtitles.push(subtitle)
        },
        onFileSelected() {
            this.files = this.$refs.myFiles.files;
            const fileElem = document.getElementById("fileElem"),
                fileWrapper = document.getElementById("fileWrapper");

                fileWrapper.innerHTML = "";
                const video = document.createElement("video");
                video.src = URL.createObjectURL(this.files[0]);
                video.height = 300;
                video.controls="controls";
                video.onload = function() {
                    URL.revokeObjectURL(this.src);
                }
                fileWrapper.appendChild(video);
                fileElem.style.display = "none";
        },
    }
}
</script>

<style lang="scss" scoped>
    @mixin flex_center() {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .content-row{
        display: flex;
    }
    .subtitles{
        width: 50%;
        &__top{
            @include flex_center();
        }
        &__bottom{
            display: flex;
            flex-direction: row;
            justify-content: flex-end;
        }
    }
    .create{
        margin: 20px;
    }
    .video{
        width: 50%;
        &__tag{
            margin: 15px 0 0 50px;
            width: 80%;
        }
        &__file-input{
            margin: 8px 50px 0px 50px;
            width: 80%;
        }
        &__input{
            width: 80%;
            @include flex_center();
        }
        &__wrapper{
            margin: 30px;
        }
    }
</style>