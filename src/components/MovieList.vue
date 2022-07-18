<template>
    <div>
        <h2>{{movieTitle}}</h2>
        <img :src="movieImg" />
        {{movieid}}
        <RouterLink :to="{ path: `/movie/${movieid}`}">Go to movie</RouterLink>
        <button @click="$emit('addFavourites', movieid)">Add to favourites</button>
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
        movieid: {
            type: Number,
            required: true
        },
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