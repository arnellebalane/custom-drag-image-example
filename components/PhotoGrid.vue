<template>
    <div class="photo-grid">
        <div
            v-for="photo in photos"
            :key="photo"
            :class="{selected: isSelected(photo)}"
            @click="toggle(photo)"
            @dragstart="onDragStart($event, photo)"
            @dragend="onDragEnd"
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
                selectedPhotos: [],
                canvas: null
            };
        },

        watch: {
            canvas(value, oldValue) {
                if (!value) {
                    oldValue.remove();
                }
            }
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

                e.dataTransfer.setDragImage(this.generateDragImage(links), 20, 20);
            },

            onDragEnd(e) {
                // If dropped on a valid drop zone, dropEffect will not be none
                if (e.dataTransfer.dropEffect !== 'none') {
                    this.selectedPhotos = [];
                }

                this.canvas = null;
            },

            generateDragImage(links) {
                const images = links.split(/\n*#\n*/g).map(link => {
                    const image = new Image();
                    image.src = link;
                    return image;
                });

                const canvas = document.createElement('canvas');
                canvas.width = 120;
                const context = canvas.getContext('2d');

                if (images.length === 1) {
                    canvas.height = 80;

                    context.drawImage(images[0], 0, 0, 120, 80);
                } else {
                    const isEven = images.length % 2 === 0;
                    canvas.height = isEven
                        ? Math.ceil(images.length / 2) * 40
                        : (Math.ceil(images.length / 2) + 1) * 40;

                    images.forEach((image, i) => {
                        let x, y;
                        let width = 60;
                        let height = 40;

                        if (isEven) {
                            x = i % 2;
                            y = Math.floor(i / 2);
                        } else {
                            if (i === 0) {
                                x = 0;
                                y = 0;
                                width = 120;
                                height = 80;
                            } else {
                                x = (i - 1) % 2;
                                y = Math.floor((i - 1) / 2) + 2;
                            }
                        }

                        context.drawImage(image, 60 * x, 40 * y, width, height);
                    });
                }


                this.canvas = canvas;
                this.$el.appendChild(canvas);
                canvas.style.backgroundColor = 'transparent';
                canvas.style.position = 'absolute';
                canvas.style.top = '-100vh';
                canvas.style.left = '-100vw';

                return canvas;
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
        pointer-events: none;
    }
</style>
