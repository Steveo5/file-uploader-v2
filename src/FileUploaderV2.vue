<template>
    <div class="file-uploader">
        <h2>{{ title }}</h2>
        <div class="files-container">
            <div class="drag-file-container">
                <div class="drag-file-content">
                    <div class="drag-file-inner"
                         @dragover.prevent="onDragOver"
                         @dragleave.prevent="onDragEnd"
                         @drop.stop.prevent="onDrop"
                         :class="{ 'is-dragging-over': isDraggingOver }">
                        <svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="cloud-upload-alt" class="svg-inline--fa fa-cloud-upload-alt fa-w-20" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 640 512"><path fill="#4390bc" d="M537.6 226.6c4.1-10.7 6.4-22.4 6.4-34.6 0-53-43-96-96-96-19.7 0-38.1 6-53.3 16.2C367 64.2 315.3 32 256 32c-88.4 0-160 71.6-160 160 0 2.7.1 5.4.2 8.1C40.2 219.8 0 273.2 0 336c0 79.5 64.5 144 144 144h368c70.7 0 128-57.3 128-128 0-61.9-44-113.6-102.4-125.4zM393.4 288H328v112c0 8.8-7.2 16-16 16h-48c-8.8 0-16-7.2-16-16V288h-65.4c-14.3 0-21.4-17.2-11.3-27.3l105.4-105.4c6.2-6.2 16.4-6.2 22.6 0l105.4 105.4c10.1 10.1 2.9 27.3-11.3 27.3z"></path></svg>
                        <input type="file" ref="file" @change="onFormInput" multiple>
                        <p>Drag and drop or <span @click="$refs.file.click()">browse</span> files to upload</p>
                    </div>
                </div>
            </div>
            <div class="uploaded-files">
                <div v-for="(file, index) in files" :key="index">
                    <div class="file-upload">
                        <!--<img v-if="file.type.includes('svg')" src="'/images/icons/svg.svg'" />-->
                        <!--<img v-else-if="file.type.includes('gzip')" src="'/images/icons/gzip.svg'" />-->
                        <!--<img v-else="file.type.includes('svg')" src="'/images/icons/blank-file.svg'" />-->
                        <svg version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 317.001 317.001" style="enable-background:new 0 0 317.001 317.001;" fill="#3490dc" xml:space="preserve"> <path d="M270.825,70.55L212.17,3.66C210.13,1.334,207.187,0,204.093,0H55.941C49.076,0,43.51,5.566,43.51,12.431V304.57 c0,6.866,5.566,12.431,12.431,12.431h205.118c6.866,0,12.432-5.566,12.432-12.432V77.633 C273.491,75.027,272.544,72.51,270.825,70.55z M55.941,305.073V12.432H199.94v63.601c0,3.431,2.78,6.216,6.216,6.216h54.903l0.006,222.824H55.941z"/></svg>

                        <div class="file-details">
                            <p>{{ file.name }}</p>
                            <div class="file-upload-bar">
                                <div class="file-upload-filled"
                                     :class="'bar-' + file.status"
                                     :style="'transform: scaleX(' + file.uploadPercentage / 100 + ');'"></div>
                            </div>
                            <div class="file-upload-size">
                                <span>{{ computedBytes(file.size) }}</span>
                                <span class="file-upload-status" :class="'status-' + file.status">
                                    <span v-if="file.status === 'ready'">Ready to upload</span>
                                    <span v-if="file.status === 'error'">Error: {{ file.errorMessage }}</span>
                                    <span v-if="file.status === 'uploading'">Uploading: {{ file.uploadPercentage }}%</span>
                                    <span v-if="file.status === 'complete'">Upload finished</span>
                                </span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <!--<div class="upload-button-container">-->
            <!--<input type="file" ref="file" @change="onFormInput">-->
            <!--<div class="upload-button">-->
            <!--Begin upload-->
            <!--</div>-->
            <!--</div>-->
        </div>
    </div>
</template>

<style lang="scss">
    .file-uploader {
        font-family: Lato;
        color: #313131;
        max-width: 800px;

        input {
            display: none;
        }

        .file-upload {
            display: flex;
            align-items: center;
            margin-top: 10px;

            svg {
                margin-right: 15px;
                width: 50px;
            }

            p {
                font-weight: bold;
                font-size: 14px;
                margin-bottom: 5px;
            }

            .file-upload-size {
                display: flex;
                justify-content: space-between;
            }

            .file-details {
                flex-grow: 1;
            }

            .file-upload-bar {
                height: 5px;
                background-color: #ededed;
                border-radius: 25px;

                .file-upload-filled {
                    min-width: 1%;
                    height: 100%;
                    background-color: #4390bc;
                    border-radius: 25px;
                    transform-origin: bottom left;
                    transition: transform 3s;

                    // Upload bar colors
                    &.bar-complete {
                        background-color: #95dfad;
                    }

                    &.bar-error {
                        background-color: #E8635C;
                    }
                }
            }

            p,
            span {
                color: #697585;
            }

            span {
                margin-top: 5px;
                font-size: 12px;
                font-weight: bold;

                &.file-upload-status {
                    span {
                        color: #4390bc;
                    }


                    // Upload status colors
                    &.status-error {
                        span {
                            color: #E8635C;
                        }
                    }

                    &.status-complete {
                        span {
                            color: #00a86b;
                        }
                    }
                }
            }
        }

        .upload-button {
            background-color: #4390bc;
            padding: 10px 20px;
            color: white;
            margin-top: 10px;
            border-radius: 25px;
            cursor: pointer;
            transition: background-color 0.3s;

            &:hover {
                background-color: #68a7ca;
            }
        }

        .upload-button-container {
            display: flex;
            justify-content: center;
        }

        h2 {
            font-weight: 300;
            border-bottom: 1px solid lightgray;
            padding-bottom: 20px;
        }

        .files-container {
            .drag-file-container {
                background-color: #fafafa;
                margin-bottom: 30px;

                .drag-file-content {
                    position: relative;
                    padding-top: 22%;
                }

                .drag-file-inner {
                    position: absolute;
                    left: 0;
                    right: 0;
                    bottom: 0;
                    top: 0;
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    flex-direction: column;
                    border: 2px dashed lightgray;
                    border-radius: 0;
                    transition: border-color 0.3s;

                    p {
                        color: #697585;

                        span {
                            color: #68a7ca;
                            cursor: pointer;

                            &:hover {
                                text-decoration: underline;
                            }
                        }
                    }

                    svg {
                        width: 70px;
                    }


                    &.is-dragging-over {
                        border-color: #3490dc;
                    }
                }
            }
        }
    }
</style>

<script>
    import axios from 'axios';

    export default {
        props: {
            postUrl: {
                type: String,
                required: true
            },
            minFileSize:  {
                type: Number,
                required: false,
                default: 10
            },
            minFileSizeMessage: {
                type: String,
                required: false,
                default: 'The file size needs to be at-least 10kb'
            },
            maxFileSize: {
                type: Number,
                required: false,
                default: 1000
            },
            maxFileSizeMessage: {
                type: String,
                required: false,
                default: 'The max file size is 100kb'
            },
            allowedFileTypes: {
                type: Array,
                required: false,
                default: () => ['image/jpeg', 'image/png', 'image/jpg']
            },
            unknownFileMessage: {
                type: String,
                required: false,
                default: 'Only images are allowed'
            },
            title: {
                type: String,
                required: false,
                default: 'Add files'
            }
        },
        data() {
            return {
                isDraggingOver: false,
                files: [],
                currentUploadPercentage: 0
            }
        },
        methods: {
            handleFileUpload(files){
                // Push all files with extra data to the file lists
                for (let i = 0; i < files.length; i++) {
                    const file = files[i]
                    this.files.push({
                        name: file.name,
                        size: file.size,
                        uploaded: 0,
                        type: file.type,
                        uploadPercentage: 0,
                        status: 'ready',
                        errorMessage: '',
                        data: file
                    })
                }
            },
            // Will be fired by our '@drop.stop.prevent'; in this case, when a file is dropped over our app
            onDrop(e) {
                const files = e.dataTransfer.files;
                this.handleFileUpload(files)
                this.onDragEnd()
                this.uploadNextInQueue()
            },
            uploadNextInQueue() {
                for (let i = 0; i < this.files.length; i++) {
                    let file = this.files[i]

                    // File is ready for upload and should be processed now
                    if (file.status === 'ready') {
                        const config = {
                            onUploadProgress: (progressEvent) => {
                                let percentCompleted = Math.round((progressEvent.loaded * 100) / progressEvent.total)
                                file.uploadPercentage = percentCompleted
                            },
                            headers: {
                                'Content-Type': 'multipart/form-data'
                            }
                        }

                        file.status = 'uploading'

                        // Check the minimum file size
                        if (file.size / 1000 < this.minFileSize) {
                            file.status  = 'error'
                            file.errorMessage = this.minFileSizeMessage
                        }

                        // Check the maximum file size
                        if (file.size / 1000 > this.maxFileSize) {
                            file.status  = 'error'
                            file.errorMessage = this.maxFileSizeMessage
                        }

                        if (!this.allowedFileTypes.includes(file.type)) {
                            file.status  = 'error'
                            file.errorMessage = this.unknownFileMessage
                        }

                        // After 100ms set upload percentage to 100, so we get the bar animation
                        if (file.status == 'error') {
                            setTimeout(() => {
                                file.uploadPercentage = 100
                            }, 100)

                            continue;
                        }

                        this.currentUploadPercentage = 0
                        let data = new FormData()
                        data.append('file', file.data)

                        axios.post(this.postUrl, data, config)
                            .then(res => {
                                file.status = 'complete';

                                this.$emit('uploaded', { 'file': file, 'response': res.data })
                            })
                            .catch(err => {
                                file.status = 'error'

                                switch(err.response.status) {
                                    default:
                                        file.errorMessage = err.response.statusText
                                        break;
                                }
                            })
                            .finally(() => {
                                this.uploadNextInQueue()
                            })

                        break;
                    }
                }
            },
            onFormInput(e) {
                const files = e.target.files;
                this.handleFileUpload(files)
                this.uploadNextInQueue()
            },
            onDragOver() {
                this.isDraggingOver = true
            },
            onDragEnd() {
                this.isDraggingOver = false
            },
            computedBytes(bytes) {
                let MB = parseInt((bytes / 1000000).toFixed(0))
                let KB = parseInt((bytes / 1000).toFixed(0))

                if (MB == 0) {
                    return KB + ' KB'
                } else {
                    return MB + ' MB'
                }
            }
        }
    }
</script>