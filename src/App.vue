<template>
  <div class="row containerApp">
    <div class="p-4 containerLeft" :style="leftContainerStyle" @click="adjustLayout('leftClicked')">
      <input class="form-control" type="search" v-model="searchQuery" @input="debouncedSearch" placeholder="Search for a movie or series" aria-label="Search">
    </div>
    <div class="p-4 containerRight" :style="rightContainerStyle" @click="adjustLayout('rightClicked')">
      <div class="row" style="height: 20%">
        <div class="col">

        </div>
        <div class="col">
          <Toggle-Btn>k</Toggle-Btn>
        </div>
      </div>
      <div class="row " style="height: 80%">
        <div class="col scrollable-container">
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
    </div>
  </div>
</template>

<script>
import { debounce } from './utils';
import DataBaseEntry from '@/components/DataBaseEntry.vue';
import ToggleBtn from '@/components/ToggleBtn.vue';

export default {
  name: 'App',
  data() {
    return {
      searchQuery: '',
      searchResults: [],
      movies: [],
      allMovies: [],
      leftContainerStyle: {
        width: '58.333333%',
        transition: 'width 0.4s ease'
      },
      rightContainerStyle: {
        width: '41.666667%',
        transition: 'width 0.4s ease'
      }
    }
  },
  components: {
    DataBaseEntry,
    ToggleBtn
  },
  methods: {

    sortMoviesByRanking() {
      this.movies.sort((a, b) => b.rank - a.rank); // Beispiel, wenn 'rank' die zu sortierende Eigenschaft ist
    },

    adjustLayout(action) {
      if (action === 'leftClicked') {
        this.leftContainerStyle.width = '58.333333%';
        this.rightContainerStyle.width = '41.666667%';
      } else {
        this.leftContainerStyle.width = '41.666667%';
        this.rightContainerStyle.width = '58.333333%';
      }
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
  .containerApp
  {
    border-radius: 20px;
    overflow: hidden;
    height: 80%;
  }

  .containerLeft
  {
    background-image: linear-gradient(to right, #9f80f4, #f96cab);
  }

  .containerRight
  {
    background-image: linear-gradient(to right, #7784b0, #384162);
  }



 .appContainer{
    backdrop-filter: blur(10px);
    color:white;
    padding: 20px;
    border-radius: 15px;
 }

  .scrollable-container {
  max-height: 600px;
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
