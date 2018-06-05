<template>
    <div class="photo-grid">
        <div
            v-for="photo in photos"
            :key="photo"
            :class="{selected: isSelected(photo)}"
            @click="toggle(photo)"
            @dragstart="onDragStart($event, photo)"
            class="photo"
            draggable="true"
        >
            <img :src="photo" alt="Photo grid item" />
        </div>
    </div>
</template>

<script>
    export default {
        name: 'PhotoGrid',

        data() {
            return {
                photos: [
                    require('../images/01.jpg'),
                    require('../images/02.jpg'),
                    require('../images/03.jpg'),
                    require('../images/04.jpg'),
                    require('../images/05.jpg'),
                    require('../images/06.jpg'),
                    require('../images/07.jpg'),
                    require('../images/08.jpg'),
                    require('../images/09.jpg'),
                    require('../images/10.jpg'),
                ],
                selectedPhotos: []
            };
        },

        methods: {
            toggle(photo) {
                if (this.isSelected(photo)) {
                    this.deselect(photo);
                } else {
                    this.select(photo);
                }
            },

            select(photo) {
                if (!this.isSelected(photo)) {
                    this.selectedPhotos = [...this.selectedPhotos, photo];
                }
            },

            deselect(photo) {
                if (this.isSelected(photo)) {
                    this.selectedPhotos = this.selectedPhotos.filter(p => p !== photo);
                }
            },

            isSelected(photo) {
                return this.selectedPhotos.includes(photo);
            },

            onDragStart(e, photo) {
                this.select(photo);

                const links = this.selectedPhotos
                    .map(link => window.location.origin + link)
                    .join('\n#\n');

                e.dataTransfer.setData('text/uri-list', links);
                e.dataTransfer.setData('text/plain', links);
            }
        }
    };
</script>

<style scoped>
    .photo-grid {
        display: flex;
        flex-wrap: wrap;
        align-items: center;
        padding: 5px;
    }

    .photo {
        width: calc(var(--photo-width, 150px) - 10px);
        margin: 5px;
        border-radius: 3px;
        position: relative;
        cursor: pointer;
    }

    .photo.selected::before {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        border: 5px solid #2196f3;
        border-radius: 3px;
        pointer-events: none;
    }

    img {
        display: block;
        width: 100%;
        border-radius: 3px;
    }
</style>
