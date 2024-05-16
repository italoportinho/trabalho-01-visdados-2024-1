---
toc: false
---

<style>

.hero {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: var(--sans-serif);
  margin: 4rem 0 8rem;
  text-wrap: balance;
  text-align: center;
}

.hero h1 {
  margin: 2rem 0;
  max-width: none;
  font-size: 14vw;
  font-weight: 900;
  line-height: 1;
  background: linear-gradient(30deg, var(--theme-foreground-focus), currentColor);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero h2 {
  margin: 0;
  max-width: 34em;
  font-size: 20px;
  font-style: initial;
  font-weight: 500;
  line-height: 1.5;
  color: var(--theme-foreground-muted);
}

@media (min-width: 640px) {
  .hero h1 {
    font-size: 90px;
  }
}

</style>

<div class="hero">
  <h1>Trabalho 1 Visualização de Dados 2024-1</h1>
  <h2>Professor: Marcos Lage </br> Grupo: Italo Portinho, Lyncoln</h2>  
</div>

## Song Metadata and Streaming Data Overview

This dataset provides comprehensive information about various songs and their performance on different music streaming platforms. Below is the breakdown of each field in the dataset:

- **track_name**: Name of the song.
- **artist(s)_name**: Name(s) of the artist(s) involved in the song.
- **artist_count**: Number of artists contributing to the song.
- **released_year**: Year the song was released.
- **released_month**: Month the song was released.
- **released_day**: Day of the month the song was released.
- **in_spotify_playlists**: Number of Spotify playlists that include the song.
- **in_spotify_charts**: Presence and rank of the song on Spotify charts.
- **streams**: Total number of streams the song has on Spotify.
- **in_apple_playlists**: Number of Apple Music playlists that include the song.
- **in_apple_charts**: Presence and rank of the song on Apple Music charts.
- **in_deezer_playlists**: Number of Deezer playlists that include the song.
- **in_deezer_charts**: Presence and rank of the song on Deezer charts.
- **in_shazam_charts**: Presence and rank of the song on Shazam charts.
- **bpm**: Beats per minute, indicating the tempo of the song.
- **key**: Musical key of the song.
- **mode**: Mode of the song (major or minor).
- **danceability_%**: Percentage indicating how suitable the song is for dancing.
- **valence_%**: Measure of the musical positivity conveyed by the song.
- **energy_%**: Perceived energy level of the song.
- **acousticness_%**: Proportion of acoustic sounds in the song.
- **instrumentalness_%**: Proportion of instrumental sounds in the song.
- **liveness_%**: Presence of live recorded elements.
- **speechiness_%**: Amount of spoken word in the song.