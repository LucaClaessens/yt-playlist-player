<script>
  import VideoSnippet from "./VideoSnippet.svelte";
  import { playlist } from "./stores.js";
</script>

<style>
  .playlist {
    margin-top: 8px;
    border-bottom: 1px solid #ccc;
    padding: 8px;
  }
  h4 {
    margin-top: 0px;
    font-size: 0.9rem;
    color: #333;
  }
  .playlist-player-results {
    max-height: 200px;
    overflow-y: auto;
  }
</style>

<div class="playlist">
  <h4>Afspeellijst</h4>
  <div class="playlist-player-results">
    {#each $playlist as video (video.etag)}
      <VideoSnippet data={video} action="remove" thumbnailSize="default" />
    {/each}
  </div>

</div>
