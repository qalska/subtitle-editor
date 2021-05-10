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
                  <v-simple-table>
                    <template v-slot:default>
                        <thead>
                            <tr>
                                <th class="text-left">
                                    №
                                </th>
                                <th class="text-left">
                                    Start
                                </th>
                                <th class="text-left">
                                    End
                                </th>
                                <th class="text-left">
                                    Text
                                </th>
                                <th class="text-left">
                                </th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr
                            v-for="(subtitle, idx) of subtitles"
                            :key="subtitle.id"
                            v-show="isEditing = true">
                                <td class="subtitles__bottom-id" scope="row">
                                    {{ idx + 1 }}
                                </td>
                                <td class="subtitles__bottom-time">
                                    {{ subtitle.startTime }}
                                </td>
                                <td class="subtitles__bottom-time">
                                    {{ subtitle.endTime }}
                                </td>
                                <td class="subtitles__bottom-text">
                                    <div class="subtitles__bottom-wrapper">{{ subtitle.text }}</div>
                                </td>
                                <td>
                                    <div class="subtitles__bottom-actions">
                                        <v-btn icon text class="subtitles__bottom-edit" v-on:click="isEditing = true">
                                            <v-icon>{{ icons.mdiPencil }}</v-icon>
                                        </v-btn>
                                        <v-btn icon text class="subtitles__bottom-delete"
                                        @click="deleteSubtitle()">
                                            <v-icon>{{ icons.mdiDelete }}</v-icon>
                                        </v-btn>
                                    </div>
                                </td>
                            </tr>
                            <tr v-show="!isEditing"
                            v-for="(subtitle, idx) of subtitles"
                            v-bind:key="idx">
                                <td>
                                    <v-text-field
                                    type="time"
                                    id="start-time"
                                    v-model="subtitle.startTime" 
                                    step="1" 
                                    required
                                    ></v-text-field>
                                </td>
                                <td>
                                    <v-text-field
                                    type="time"
                                    id="end-time"
                                    v-model="subtitle.endTime" 
                                    step="1" 
                                    required
                                    ></v-text-field>
                                </td>
                                <td>
                                    <v-text-field
                                    type="text"
                                    id="text1"
                                    v-model="subtitle.text" 
                                    required
                                    ></v-text-field>
                                </td>
                                <td>
                                    <v-btn
                                        text
                                        color="error"
                                        type="button"
                                        v-on:click="isEditing = false">
                                        Close
                                    </v-btn>
                                </td>
                            </tr>
                        </tbody>
                    </template>
                </v-simple-table>
            </div>
        </div>
        <div class="video">
            <div class="video__input">
                <v-file-input
                    accept="video/*"
                    label="Upload video"
                ></v-file-input>
                <div class="video__wrapper" id="fileWrapper">
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import {
    mdiPencil,
    mdiDelete,
  } from '@mdi/js'
import SubtitleCreatorDialog from '@/components/SubtitleCreatorDialog'
export default {
    components: {
        SubtitleCreatorDialog,
    },
    data() {
        return {
            subtitles: [],
            files: [],
            isEditing: false,
            icons: {
                mdiPencil,
                mdiDelete,
            },
        }
    },
    methods: {
        addSubtitle(subtitle) {
            this.subtitles.push(subtitle)
        },
        deleteSubtitle(id) {
            this.subtitles.splice(id, 1);
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
                width: 250px;
            }
            &-actions{
                display: flex;
                flex-direction: row;
                justify-content: flex-end;
                width: 60px;
            }
            &-delete{
                margin-left: 10px;
            }
            &-wrapper{
                width: 250px;
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