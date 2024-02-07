<template>
  <div class="d-flex align-items-center justify-content-center">
      <div class="text-center appContainer">
          <!-- Logo -->
          <div class="row mb-3" style="width: 800px;">
            </div>

            <!-- Search Bar -->
            <div class="row mb-4">
                <div class="col">
                    <input class="form-control" type="search" v-model="searchQuery" @input="debouncedSearch" placeholder="Search for a movie or series" aria-label="Search">
                    <div v-if="searchResults.length" class="dropdown-menu d-block">
                        <a v-for="(result, index) in searchResults" :key="index" class="dropdown-item" href="#">
                            {{ result.title }}
                        </a>
                    </div>
                </div>
            </div>

          <!-- Sort Button -->
          <button class="btn btn-primary mb-4" @click="sortMoviesByRanking">Sort by Ranking</button>

          <!-- 6x6 Card Grid (angepasst für Scrollbar) -->
          <div class="row">
              <div class="col">
                <h4>Movies</h4>
                <div class="container scrollable-container gap-3">
                  <data-base-entry class="mb-3" watchCounter="2" addDate="2.2.2" title="The Movie"></data-base-entry>
                  <data-base-entry class="mb-3" watchCounter="2" addDate="2.2.2" title="The Movie"></data-base-entry>
                  <data-base-entry class="mb-3" watchCounter="2" addDate="2.2.2" title="The Movie"></data-base-entry>
                  <data-base-entry class="mb-3" watchCounter="2" addDate="2.2.2" title="The Movie"></data-base-entry>
                  <data-base-entry class="mb-3" watchCounter="2" addDate="2.2.2" title="The Movie"></data-base-entry>                  
                  <data-base-entry class="mb-3" watchCounter="2" addDate="2.2.2" title="The Movie"></data-base-entry>                  
                  <data-base-entry class="mb-3" watchCounter="2" addDate="2.2.2" title="The Movie"></data-base-entry>                  
                  <data-base-entry class="mb-3" watchCounter="2" addDate="2.2.2" title="The Movie"></data-base-entry>                  
                  <data-base-entry class="mb-3" watchCounter="2" addDate="2.2.2" title="The Movie"></data-base-entry>                  
                </div>
              </div>
              <div class="col">
                <h4>TV-Shows</h4>
                <div class="container scrollable-container">
                  <data-base-entry class="mb-3" watchCounter="2" addDate="2.2.2" title="The Movie"></data-base-entry>
                  <data-base-entry class="mb-3" watchCounter="2" addDate="2.2.2" title="The Movie"></data-base-entry>
                  <data-base-entry class="mb-3" watchCounter="2" addDate="2.2.2" title="The Movie"></data-base-entry>
                  <data-base-entry class="mb-3" watchCounter="2" addDate="2.2.2" title="The Movie"></data-base-entry>
                  <data-base-entry class="mb-3" watchCounter="2" addDate="2.2.2" title="The Movie"></data-base-entry>                  
                </div>
              </div>
          </div>
      </div>
  </div>
</template>

<script>
import { debounce } from './utils';
import DataBaseEntry from '@/components/DataBaseEntry.vue';

export default {
  name: 'App',
  data() {
    return {
      searchQuery: '',
      searchResults: [],
      movies: [],
      allMovies: [] 
    }
  },
  components: {
    DataBaseEntry
  },
  methods: {

    sortMoviesByRanking() {
      this.movies.sort((a, b) => b.rank - a.rank); // Beispiel, wenn 'rank' die zu sortierende Eigenschaft ist
    },

    async searchMovies() {
      if (this.searchQuery === '') {
        this.movies = this.allMovies;
        return;
      }

      // Filter client-side if data already fetched
      const filteredMovies = this.allMovies.filter(movie => 
        movie.title.toLowerCase().includes(this.searchQuery.toLowerCase())
      );

      if (filteredMovies.length > 0) {
        this.movies = filteredMovies;
      } else {
        // Fetch from API if no local match
        await this.fetchMoviesFromApi(this.searchQuery);
      }
    },
    
    debounce(func, delay) 
    {
        let debounceTimer;
        
        return function() {
        const context = this;
        const args = arguments;
        clearTimeout(debounceTimer);
        debounceTimer = setTimeout(() => func.apply(context, args), delay);
        }
    },

    async fetchMoviesFromApi(query) {
      const tmdbKey = '85491f66008acc113a6114a3a422d6da';
      const tmdbBaseUrl = 'https://api.themoviedb.org/3';
      const apiKey = '&api_key=' + tmdbKey;
      const searchEndpoint = `/search/multi?include_adult=true&query=${query}`;
      const urlToFetch = `${tmdbBaseUrl}${searchEndpoint}${apiKey}`;

      try {
        const response = await fetch(urlToFetch);
        if (response.ok) {
          const jsonResponse = await response.json();
          // Assuming 'results' contains the movies
          this.movies = jsonResponse.results;
          this.allMovies = [...this.allMovies, ...jsonResponse.results];
        }
      } catch (error) {
        console.error(error);
      }
  }
  },
  created() {
    this.debouncedSearch = debounce(this.searchMovies, 500);
  }
}
</script>

<style scoped>

 .appContainer{
    backdrop-filter: blur(10px);
    color:white; 
    padding: 20px; 
    border-radius: 15px;
 }

  .scrollable-container {
  max-height: 700px;
  overflow-y: auto; 
  scrollbar-width: thin; 
  scrollbar-color: #888 transparent;
}


.scrollable-container::-webkit-scrollbar {
  width: 4px; 
}

.scrollable-container::-webkit-scrollbar-track {
  background: transparent;
}

.scrollable-container::-webkit-scrollbar-thumb {
  background-color: #888; 
  border-radius: 2px;
}
</style>