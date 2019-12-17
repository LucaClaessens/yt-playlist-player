<script>
  import PlaylistPlayerSearchbar from "./PlaylistPlayerSearchbar.svelte";
  import Player from "./Player.svelte";
  import Playlist from "./Playlist.svelte";
  import VideoSnippet from "./VideoSnippet.svelte";

  import { searchResults, currentVideoId, playlist } from "./stores.js";

  function playerStateChange(event) {
    const { data } = event.detail;

    if (data === YT.PlayerState.ENDED) {
      const playlistSnapshot = $playlist;
      if (Array.isArray(playlistSnapshot) && playlistSnapshot.length > 0) {
        const nextVideoId = playlistSnapshot[0].id.videoId;
        currentVideoId.update(id => nextVideoId);
        playlist.update(p => [...playlistSnapshot.splice(1)]);
      }
    }
  }
</script>

<style>
  .playlist-player {
    display: flex;
  }
  .playlist-player-content {
    max-width: 600px;
    margin-right: 15px;
  }
  .search-results {
    margin-top: 8px;
    padding: 16px;
    background-color: #f3f3f3;
    width: 400px;
    max-height: 80vh;
    overflow-y: auto;
    border-radius: 2px;
  }
  p.no-results {
    margin-top: 0;
    color: #333;
    font-size: 0.8rem;
    text-align: center;
  }
</style>

<div class="playlist-player">
  <div class="playlist-player-content">
    <PlaylistPlayerSearchbar />
    <Player videoId={currentVideoId} on:stateChange={playerStateChange} />
  </div>
  <aside>
    {#if $playlist.length > 0}
      <Playlist />
    {/if}
    <div class="search-results">
      {#if $searchResults.length > 0}
        {#each $searchResults as result (result.etag)}
          <VideoSnippet data={result} action="add" />
        {/each}
      {:else}
        <p class="no-results">(Nog) geen resultaten gevonden!</p>
      {/if}
    </div>
  </aside>
</div>
