<template>
    <div class="app" id="app">
        <div class="wrapper">
            <header>
                <h1>Custom Drag Image Example</h1>
                <a href="https://github.com/arnellebalane/custom-drag-image-example" target="_blank" rel="noopener noreferrer">
                    github.com/arnellebalane/custom-drag-image-example
                </a>
            </header>

            <main>
                <PhotoGrid />
                <PhotoList />
            </main>
        </div>
    </div>
</template>

<script>
    import PhotoGrid from './components/PhotoGrid.vue';
    import PhotoList from './components/PhotoList.vue';

    export default {
        name: 'App',

        components: {
            PhotoGrid,
            PhotoList
        }
    };
</script>

<style scoped>
    .wrapper {
        width: 100%;
        max-width: 800px;
        margin: 0 auto;
    }

    header {
        margin-bottom: 50px;
        text-align: center;
    }

    header h1 {
        font-size: 24px;
    }

    header a {
        font-size: 14px;
        color: #888;
    }

    main {
        display: flex;
    }

    .photo-grid,
    .photo-list {
        flex: 1 0 0;
    }

    .photo-grid {
        --photo-width: 50%;
        margin-right: 50px;
    }
</style>
