<template lang="pug">
.search-bar
  .input-wrapper
    img.icon-magnifier(src="./icons/icon-magnifier.png")
    input(type="search" placeholder="Search..." v-model="searchQuery" @input="handleSearch")
  ul.search-results
    .loading-spinner(v-if="isLoading")
      img.loading-image(src="./icons/loading.svg")
    .result-wrapper(v-else)
      template(v-if="searchResults.albums.length")
        .category-heading Albums
        li(v-for="(album, index) in searchResults.albums" :key="index") {{ album.collectionName }}
      template(v-if="searchResults.artists.length")
        .category-heading Artists
        li(v-for="(artist, index) in searchResults.artists" :key="index") {{ artist.artistName }}
      template(v-if="searchResults.songs.length")
        .category-heading Songs
        li(v-for="(song, index) in searchResults.songs" :key="index") {{ song.trackName }}
</template>

<script setup lang="ts">
import { ref } from 'vue'

const searchQuery = ref('')

defineProps({
  searchResults: {
    type: Object as () => SearchResults,
    default: () => ({
      albums: [],
      artists: [],
      songs: [],
    }),
  },
  loading: {
    type: Boolean,
    default: false,
  },
})

const emit = defineEmits(['perform-search'])

interface SearchResults {
  albums: any[]
  artists: any[]
  songs: any[]
}

function handleSearch() {
  emit('perform-search', searchQuery.value)
}
</script>

<style scoped>
.search-bar {
  position: relative;
  width: 500px;
  margin: 2rem auto;
}

.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

.icon-magnifier {
  position: absolute;
  left: 5px;
  width: 20px;
  bottom: 10px;
}

.loading-spinner {
  display: flex;
  justify-content: center;
  align-items: center;
}

.loading-image {
  width: 50px;
  height: 50px;
}

input[type='search'] {
  width: 100%;
  padding: 10px 10px 10px 30px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

input[type='search']:focus {
  outline: none;
  border-color: #66afe9;
}

ul.search-results {
  position: absolute;
  top: calc(100% + 1px);
  left: 0;
  width: 100%;
  overflow-y: auto;
  background-color: #fff;
  border: 1px solid #908585;
  border-radius: 0 0 4px 4px;
  padding: 0;
  list-style: none;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
}

ul.search-results li {
  padding: 10px;
  cursor: pointer;
}

ul.search-results li:hover {
  background-color: #f5f5f5;
}

.category-heading {
  font-size: 14px;
  font-weight: lighter;
  margin: 5px 5px;
}
</style>
