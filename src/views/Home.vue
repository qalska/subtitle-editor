<template>
    <div class="content-row">
        <div class="subtitles">
            <div class="subtitles__top">
                <subtitle-creator-dialog
                @add-subtitle="addSubtitle"
                />
            </div>
            <hr>
            <div class="subtitles__bottom">
                <table class="table subtitles__bottom-table">
                    <thead>
                        <tr>
                            <th scope="col">№</th>
                            <th scope="col">Start</th>
                            <th scope="col">End</th>
                            <th scope="col">Text</th>
                            <th scope="col"></th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr
                        v-for="(subtitle, idx) of subtitles"
                        :key="subtitle.id"
                        >
                            <td class="subtitles__bottom-id" scope="row">{{idx + 1}}</td>
                            <td class="subtitles__bottom-time">{{subtitle.startTime}} </td>
                            <td class="subtitles__bottom-time">{{subtitle.endTime}}</td>
                            <td class="subtitles__bottom-text"><div class="subtitles__bottom-wrapper">{{subtitle.text}}</div></td>
                            <td class="subtitles__bottom-actions">
                                <button class="subtitles__bottom-edit" type="submit">
                                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pencil" viewBox="0 0 16 16">
                                <path d="M12.146.146a.5.5 0 0 1 .708 0l3 3a.5.5 0 0 1 0 .708l-10 10a.5.5 0 0 1-.168.11l-5 2a.5.5 0 0 1-.65-.65l2-5a.5.5 0 0 1 .11-.168l10-10zM11.207 2.5 13.5 4.793 14.793 3.5 12.5 1.207 11.207 2.5zm1.586 3L10.5 3.207 4 9.707V10h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.293l6.5-6.5zm-9.761 5.175-.106.106-1.528 3.821 3.821-1.528.106-.106A.5.5 0 0 1 5 12.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.468-.325z"/>
                                </svg>
                                </button>
                                <button class="subtitles__bottom-delete" type="button"
                                @click="deleteSubtitle()">
                                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16">
                                <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"/>
                                <path fill-rule="evenodd" d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"/>
                                </svg>
                                </button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <!-- <div class="subtitles__bottom">
                <a 
                href="#" 
                download="sub.srt" 
                class="btn"
                id="link"
                @click="onLoadSubtitles()"
                >Download subtitles</a>
            </div> -->
        </div>
        <div class="video">
            <div class="video__input">
                <div class="field__wrapper">
                    <input type="file" class="field field__file" id="fileElem" accept="video/*" ref="myFiles" @change="onFileSelected()">
                        
                    <label class="field__file-wrapper" for="field__file-2">
                        <div class="field__file-fake">Upload video</div>
                        <div class="field__file-button">Browse</div>
                    </label>
                </div>
                <div class="video__wrapper" id="fileWrapper">
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import SubtitleCreatorDialog from '@/components/SubtitleCreatorDialog'
export default {
    components: {
        SubtitleCreatorDialog,
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
        deleteSubtitle(id) {
            this.subtitles = this.subtitles.filter(t => t.id !== id)
        },
        onFileSelected() {
            this.files = this.$refs.myFiles.files;
            const fileElem = document.getElementById("fileElem"),
                fileWrapper = document.getElementById("fileWrapper");

                fileWrapper.innerHTML = "";
                const video = document.createElement("video");
                const track = document.createElement("track");
                track.default="default";
                track.kind="captions";
                track.srclang="en";
                track.src=""
                video.src = URL.createObjectURL(this.files[0]);
                video.height = 300;
                video.controls="controls";
                video.onload = function() {
                    URL.revokeObjectURL(this.src);
                }
                fileWrapper.appendChild(video);
                video.appendChild(track);
                fileElem.style.display = "none";
        },
    },
    watch: {
        subtitles: function() {
            const parser = require('subtitles-parser');
            const data = parser.toSrt(this.subtitles);
            let blob = new Blob([String('WEBVTT \n' + data)], {type: 'text/plain'});
            const subtitlesLink = URL.createObjectURL(blob);
            document.querySelector("track").src = subtitlesLink;
        }
    }
}
</script>

<style lang="scss" scoped>
    @import '@/scss/file-input.scss';
    @import '@/scss/mixins.scss';
    .content-row{
        display: flex;
    }
    .subtitles{
        width: 50%;
        &__bottom{
            &-id{
                width: 10px;
            }
            &-time{
                width: 30px;
            }
            &-text{
                max-width: 150px;
            }
            &-actions{
                width: 60px;
            }
            &-delete{
                margin-left: 10px;
            }
            &-wrapper{
                word-break: break-all;
            }
        }
    }
    .video{
        width: 50%;
        margin: 0 20px;
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
            @include flex_space-between();
        }
        &__wrapper{
            margin: 30px;
        }
    }
</style>