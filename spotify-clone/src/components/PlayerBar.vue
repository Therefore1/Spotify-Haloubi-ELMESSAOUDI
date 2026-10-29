<template>
  <footer>
    <!-- lecteur officiel Spotify : fonctionne même sans preview_url -->
    <iframe
      :key="track.id"
      :src="`https://open.spotify.com/embed/track/${track.id}`"
      width="100%"
      height="80"
      allow="encrypted-media"
    ></iframe>
  </footer>
</template>

<script>
export default {
  name: 'PlayerBar',
  props: {
    track: { type: Object, required: true }
  }
}
</script>
