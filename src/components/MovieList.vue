<template>
    <div>
        <h2>{{movieTitle}}</h2>
        <img :src="movieImg" />
    </div>
</template>

<script>
export default {
    name: 'MovieList',
    data() {
        return {

        }
    },
    mounted() {
        console.log('movie list is mounted');
    },
    methods: {

    },
    props: {
        movieTitle: {
            type: String,
            required: false
        },
        movieImg: {
            type: String,
            required: false
        },
    }
}
</script>


<style scoped>

</style>