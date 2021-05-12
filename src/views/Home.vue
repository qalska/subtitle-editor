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
                                :key="subtitle.startTime"
                                v-show="!subtitle.isEditing">
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
                                        <v-btn icon text class="subtitles__bottom-edit" 
                                               @click="editSubtitle(idx)">
                                            <v-icon>{{ icons.mdiPencil }}</v-icon>
                                        </v-btn>
                                        <v-btn icon text class="subtitles__bottom-delete"
                                               @click="deleteSubtitle(idx)">
                                            <v-icon>{{ icons.mdiDelete }}</v-icon>
                                        </v-btn>
                                    </div>
                                </td>
                            </tr>
                            <tr v-show="subtitle.isEditing"
                            v-for="(subtitle, idx) of subtitles"
                            :key="idx">
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
                                        @click="subtitle.isEditing = false">
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
                    @change="onFileSelected"
                    v-if="videoSrc == null"
                ></v-file-input>
                <div 
                    class="video__wrapper" 
                    id="fileWrapper"
                    v-else>
                    <video v-bind:src="videoSrc" 
                           height="300" 
                           controls>
                        <track v-bind:src="trackSrc" 
                               default
                               kind="captions"
                               srclang="en" />
                    </video>
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
            srtSrc: null,
            videoSrc: null,
            trackSrc: null,
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
            this.subtitles.splice(id, 1)
        },
        editSubtitle(id) {
            let subtitle = this.subtitles[id];
            subtitle.isEditing = true;
        },
        onFileSelected(e) {
            const file = e;
            this.videoSrc = URL.createObjectURL(file);
            file.onload = function() {
                URL.revokeObjectURL(this.videoSrc);
            }
        },
    },
    watch: {
        subtitles: function() {
            const parser = require('subtitles-parser');
            const data = parser.toSrt(this.subtitles);
            let blob = new Blob([String('WEBVTT \n' + data)], {type: 'text/plain'});
            this.trackSrc = URL.createObjectURL(blob);
        }
    }
}
</script>

<style lang="scss" scoped>
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