<template>
  <header>
    <h1>Spotify Clone</h1>
    <input
      v-model="token"
      @change="saveToken"
      type="password"
      placeholder="Colle ton token Spotify ici"
    />
  </header>

  <form class="search" @submit.prevent="search">
    <input v-model="query" placeholder="Rechercher un titre ou un artiste..." />
    <button type="submit">Rechercher</button>
  </form>

  <p class="error" v-if="error">{{ error }}</p>

  <TrackRow
    v-for="track in tracks"
    :key="track.id"
    :track="track"
    @play="current = track"
  />

  <PlayerBar v-if="current" :track="current" />
</template>

<script>
import TrackRow from './components/TrackRow.vue'
import PlayerBar from './components/PlayerBar.vue'

export default {
  name: 'App',
  components: { TrackRow, PlayerBar },
  data() {
    return {
      token: localStorage.getItem('spotify_token') || '',
      query: '',
      tracks: [],
      current: null,
      error: ''
    }
  },
  methods: {
    saveToken() {
      localStorage.setItem('spotify_token', this.token)
    },
    search() {
      this.error = ''
      if (!this.token) {
        this.error = 'Ajoute ton token Spotify en haut de la page.'
        return
      }
      const url = 'https://api.spotify.com/v1/search?type=track&limit=10&q=' + encodeURIComponent(this.query)
      fetch(url, { headers: { Authorization: 'Bearer ' + this.token } })
        .then(response => {
          if (response.status === 401) throw new Error('Token invalide ou expiré (il dure 1 heure).')
          if (!response.ok) throw new Error('Erreur Spotify : ' + response.status)
          return response.json()
        })
        .then(data => this.tracks = data.tracks.items)
        .catch(err => this.error = err.message)
    }
  }
}
</script>
