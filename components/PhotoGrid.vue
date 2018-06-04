<template>
    <div class="photo-grid">
        <div v-for="photo in photos" :key="photo" class="photo">
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
                ]
            };
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
        width: 150px;
        margin: 5px;
        cursor: pointer;
        overflow: hidden;
    }

    img {
        display: block;
        width: 100%;
        transition: transform 150ms ease-out;
    }

    .photo:hover img {
        transform: scale(1.1);
    }
</style>
