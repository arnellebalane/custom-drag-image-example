<template>
    <div class="photo-list">
        <div class="dropzone" @dragover.prevent @drop.prevent="onDrop">
            <img
                v-for="photo in photos"
                :key="photo"
                :src="photo"
                alt="Dropzone photo"
            >
        </div>
    </div>
</template>

<script>
    export default {
        name: 'PhotoList',

        data() {
            return {
                photos: []
            };
        },

        methods: {
            onDrop(e) {
                if (e.dataTransfer.types.includes('text/uri-list')) {
                    const links = e.dataTransfer.getData('text/uri-list')
                        .split(/\n*#\n*/g);
                    this.photos = links;
                }
            }
        }
    };
</script>

<style scoped>
    .photo-list {
        max-height: 635px;
        padding: 10px 5px;
    }

    .dropzone {
        height: 100%;
        padding: 10px;
        border: 2px dashed #ddd;
        border-radius: 3px;
        position: relative;
        overflow: auto;
    }

    .dropzone:empty::before {
        content: "Select images from the grid and drop them here.";
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 50px;
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        text-align: center;
        color: #bbb;
    }

    .dropzone img {
        display: block;
        width: 100%;
        border-radius: 3px;
    }

    .dropzone::-webkit-scrollbar {
        width: 0;
    }

    .dropzone img:not(:first-child) {
        margin-top: 10px;
    }

    .dropzone img:not(:last-child) {
        margin-bottom: 10px;
    }
</style>
