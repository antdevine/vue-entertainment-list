<template>
    <div>
        <h2>{{favouriteTitle}}</h2>
        <button @click="$emit('removeFavourites', favouriteTitle)">Remove from favourites</button>
    </div>
</template>

<script>
export default {
    name: 'FavouritesList',
    data() {
        return {

        }
    },
    mounted() {
        console.log('favourites mounted');
    },
    methods: {

    },
    props: {
        favouriteTitle: {
            type: String,
            required: true
        }
    }
}
</script>


<style scoped>

</style>