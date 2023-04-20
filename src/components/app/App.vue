<template>
  <div class="container mx-auto px-5 py-3 shadow-lg font-monospace app">
    <AppInfo :allMoviesCount="movies.length" :favouriteMoviesCount="movies.filter(e => e.favourite).length" />
    <Box>
      <SearchPanel :updateTermHandler="updateTermHandler" />
      <AppFilter :updateFilterHandler="updateFilterHandler" :filterName="filter" />
    </Box>
    <Box class="text-center text-danger fs-2" v-if="!movies.length && !isLoading">Kinolar yo'q</Box>
    <Box v-else-if="isLoading">
      <Loader/>
    </Box>
    <MovieList v-else :movies="onFilterHandler(onSearchHandler(movies, term), filter)" @onToggle="onToggleHandler"
      @onRemove="onRemoveHandler" />
    <PaginationList v-if="movies.length >= 10" :updatePage="updatePage" :totalPages="this.totalPages" />
    <MovieAddForm @createMovie="createMovie" />
  </div>
</template>

<script>
import AppInfo from '../app-info/AppInfo.vue';
import SearchPanel from '../search-panel/SearchPanel.vue';
import AppFilter from "../app-filter/AppFilter.vue";
import MovieList from "../movie-list/MovieList.vue";
import MovieAddForm from "../movie-add-form/MovieAddForm.vue";
import PaginationList from '../pagination-list/PaginationList.vue';
import axios from "axios";

export default {
  components: { AppInfo, SearchPanel, AppFilter, MovieList, MovieAddForm, PaginationList },

  data() {
    return {
      movies: [],
      term: '',
      filter: 'all',
      isLoading: false,
      limit:10,
      page:1,
      totalPages:0,
    }
  },

  methods: {
     async createMovie(newMovie) {
      const response = await axios.post('https://jsonplaceholder.typicode.com/posts',newMovie)
      this.movies.push(response.data);
    },

    onToggleHandler({ id, prop }) {
      this.movies = this.movies.map(item => {
        if (item.id == id) {
          return { ...item, [prop]: !item[prop] }
        }
        return item;
      });
    },

    async onRemoveHandler(id) {
      try {
        const response = await axios.delete(`https://jsonplaceholder.typicode.com/posts/${id}`)
        console.log(response);
        // this.movies = this.movies.filter(e => e.id != id);
      } catch (error) {
        alert(error);
      }
    },

    onSearchHandler(arr, term) {
      if (term.length == 0) {
        return arr;
      }
      return arr.filter(e => e.name.toLowerCase().indexOf(term) > -1);
    },

    onFilterHandler(arr, filter) {
      switch (filter) {
        case "popular":
          return arr.filter(e => e.like);
          break;

        case "mostViewers":
          return arr.filter(e => e.viewers > 500);
          break;
        default:
          return arr
          break;
      }
    },

    updateTermHandler(term) {
      this.term = term;
    },

    updateFilterHandler(filter) {
      this.filter = filter;
    },

    async fetchMovie() {
      try {
        this.isLoading = true;
          const response = await axios.get("https://jsonplaceholder.typicode.com/posts",{
            params:{
              _page: this.page,
              _limit:this.limit,
            }
          });
          const newArr = response.data.map(item => ({
            id: item.id,
            name: item.title,
            viewers: item.id * 10,
            like: false,
            favourite: false,
          }));
          this.totalPages = Math.ceil(response.headers['x-total-count']/this.limit);
          this.movies = newArr;
      } catch (error) {
        alert(error);
      }finally{
        this.isLoading = false;
      }
    },

    updatePage(page){
      this.page = page;
    },
  },

  mounted() {
    this.fetchMovie();
  },

  watch: {
    page(){
      this.fetchMovie();
    }
  }
}
</script>

<style scoped>
.app {
  min-height: 90vh;
}
</style>