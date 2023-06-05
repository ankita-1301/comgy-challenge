<template lang="pug">
.cards-wrapper(v-if="lastSearchResults.length")
  .card(v-for="result in lastSearchResults")
    .image-container
      img(:src="result.artworkUrl100", alt="Result Image")
    .details
      h3.name {{ result.name }}
      p.released-date Released on {{ formattedDate(result.releasedDate) }}
</template>

<script setup lang="ts">
defineProps({
  lastSearchResults: {
    type: Array,
    default: () => [],
  },
})

function formattedDate(dateStr: string) {
  if (!dateStr) {
    return ''
  }

  const date = new Date(dateStr)
  const options: Intl.DateTimeFormatOptions = { day: 'numeric', month: 'long', year: 'numeric' }
  return date.toLocaleDateString('en-US', options)
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
  width: 80px;
  height: 80px;
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

.released-date {
  font-size: 14px;
  color: #666;
}
</style>
