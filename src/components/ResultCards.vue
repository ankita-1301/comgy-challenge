<template lang="pug">
.cards-wrapper(v-if="lastSearchResults.length")
  .card(v-for="result in lastSearchResults")
    .image-container
      img(:src="result.artworkUrl100", alt="Result Image")
    .details
      h3.name {{ result.artistName }}
      h4.collection(v-if="result.collectionName") {{ result.collectionName }}
      p.released-date(v-if="result.releaseDate") Released on {{ formattedDate(result.releaseDate) }}
</template>

<script setup lang="ts">
defineProps({
  lastSearchResults: {
    type: Array,
    default: () => [],
  },
})

function formattedDate(dateStr: string) {
  const date = new Date(dateStr)
  const options: Intl.DateTimeFormatOptions = { day: 'numeric', month: 'long', year: 'numeric' }
  return date.toLocaleDateString('en-US', options) || ''
}
</script>

<style scoped>
.card {
  display: flex;
  align-items: center;
  width: 500px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-bottom: 10px;
}

.image-container {
  flex-shrink: 0;
  margin-right: 10px;
}

.image-container img {
  object-fit: cover;
  border-radius: 4px;
}

.details {
  flex-grow: 1;
}

.name {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 5px;
}
.collection {
  font-size: 14px;
  margin-bottom: 5px;
  color: #e466ef;
}

.released-date {
  font-size: 14px;
  color: #6c6b6b;
}
</style>
