<template>
    <div id="app">
        <v-stripes base-src="sample.jpeg" />
    </div>
</template>

<script>
    import VStripes from './components/VStripes.vue'

    export default {
        name: 'app',

        components: {
            VStripes,
        },

        data() {
            return {
            }
        },
    }
</script>


<style lang="scss">
    * {
        box-sizing: border-box;
    }

    body {
        background-color: $primary-light;
        font-family: sans-serif;
    }
</style>

