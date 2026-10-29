<template>
  <div class="track" @click="$emit('play')">
    <img :src="track.album.images[0]?.url" />
    <div class="infos">
      <strong>{{ track.name }}</strong>
      <p>{{ artistes }}</p>
    </div>
    <button>▶</button>
  </div>
</template>

<script>
export default {
  name: 'TrackRow',
  props: {
    track: { type: Object, required: true }
  },
  emits: ['play'],
  computed: {
    artistes() {
      return this.track.artists.map(a => a.name).join(', ')
    }
  }
}
</script>
