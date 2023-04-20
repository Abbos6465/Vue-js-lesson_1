<template>
    <Box>
        <ul class="list-group movie-list text-uppercase">
       <MovieListItem v-for="movie in movies" :movie="movie" :key="movie.id" @onToggle="$emit('onToggle', $event)" @onRemove="$emit('onRemove',movie.id)" />
    </ul>
    </Box>
</template>

<script>
import MovieListItem from '../movie-list-item/MovieListItem.vue';

export default {
    components: { MovieListItem},

  props:{
    movies:{
        type: Array,
        required: true,
    }
  }
}
</script>

<style scoped>


.movie-list{
    background-color: white;

}
</style>