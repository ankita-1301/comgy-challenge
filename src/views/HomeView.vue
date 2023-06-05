<template lang="pug">
.main-container
  h1 Music search
  search-bar(:search-results="searchResults" :loading="isLoading" @perform-search="debouncedSearch")
  result-cards(:last-search-results="lastSearchResults")
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import SearchBar from '@/components/SearchBar.vue'
import ResultCards from '@/components/ResultCards.vue'
import { debounce } from 'lodash'

interface SearchResult {
  albums: any[]
  artists: any[]
  songs: any[]
}

const searchResults = ref<SearchResult>({
  albums: [],
  artists: [],
  songs: [],
})
const debouncedSearch = debounce(handleSearch, 300)
const itunesUrl = 'https://itunes.apple.com/search'

const isLoading = computed(() => {
  const { albums, artists, songs } = searchResults.value
  return albums.length === 0 && artists.length === 0 && songs.length === 0
})

const lastSearchResults = computed(() => [
  ...searchResults.value.artists,
  ...searchResults.value.albums,
  ...searchResults.value.songs,
])

async function handleSearch(searchQuery: string) {
  await Promise.all([
    fetchResults(searchQuery, 'musicArtist', 'artists'),
    fetchResults(searchQuery, 'album', 'albums'),
    fetchResults(searchQuery, 'song', 'songs'),
  ])
}

async function fetchResults(searchQuery: string, entity: string, category: keyof SearchResult) {
  const encodedQuery = searchQuery.replace(/\s/g, '+')
  const apiUrl = new URL(itunesUrl)
  const params = new URLSearchParams({
    term: encodedQuery,
    media: 'music',
    entity,
    limit: '3',
  })
  apiUrl.search = params.toString()

  try {
    const response = await fetch(apiUrl)
    const data = await response.json()

    searchResults.value[category] = data.results
  } catch (error) {
    console.error('Error fetching search results:', error)
  }
}
</script>

<style scoped>
.main-container {
  margin: 2rem auto;
}
</style>
