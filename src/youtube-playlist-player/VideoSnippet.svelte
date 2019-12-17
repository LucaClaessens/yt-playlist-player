<script>
  import { createEventDispatcher } from "svelte";
  import { playlist, currentVideoId } from "./stores.js";

  export let data;
  export let thumbnailSize = "medium";
  export let action = "add";

  let hovered = false;
  const thumb = data.snippet.thumbnails.medium;
  const title = data.snippet.title;
  const channelTitle = data.snippet.channelTitle;

  const dispatch = createEventDispatcher();

  function updatePlaylist(event) {
    event.preventDefault();
    event.stopPropagation();
    if (action === "add") {
      if (!$playlist.some(entry => entry.id.videoId === data.id.videoId)) {
        playlist.update(p => [...p, data]);
      }
    } else if (action === "remove") {
      const sliced = $playlist.filter(
        entry => entry.id.videoId !== data.id.videoId
      );
      playlist.update(p => sliced);
    }
    return void 0;
  }

  function playVideo(event) {
    currentVideoId.update(id => data.id.videoId);
  }
</script>

<style>
  .video-snippet {
    display: flex;
    max-width: 400px;
    cursor: pointer;
    position: relative;
  }
  .video-snippet-thumb {
    position: relative;
    margin-bottom: 8px;
    margin-right: 8px;
  }
  .video-snippet-thumb.thumb-medium {
    width: 200px;
  }
  .video-snippet-thumb.thumb-default {
    width: 120px;
  }
  .video-snippet-thumb.thumb-medium > img {
    width: 200px;
  }
  .video-snippet-thumb.thumb-default > img {
    width: 120px;
  }
  .video-snippet-meta > h5 {
    margin-top: 0;
    margin-bottom: 4px;
  }
  .video-snippet-meta > p {
    margin-top: 0;
    margin-bottom: 4px;
    font-size: 0.8rem;
  }
  .playlist-play-indicator {
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
    pointer-events: none;
    z-index: 1;
  }

  .playlist-play-indicator > svg {
    fill: white;
    opacity: 0.66;
    width: 32px;
    height: 32px;
  }

  .playlist-action {
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
    top: 8px;
    right: 8px;
    padding: 6px;
    border-radius: 2px;
    background-color: rgba(0, 0, 0, 0.5);
    z-index: 1;
  }
  .playlist-action:hover {
    opacity: 0.5;
  }
  .playlist-action > svg {
    fill: white;
  }
</style>

<div
  class="video-snippet"
  title="Video afspelen"
  on:click={playVideo}
  on:mouseenter={e => (hovered = true)}
  on:mouseleave={e => (hovered = false)}>
  {#if hovered}
    <div
      class="playlist-action"
      on:click={updatePlaylist}
      title="Toevoegen aan afspeellijst">
      {#if action === 'add'}
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24">
          <path d="M0 0h24v24H0z" fill="none" />
          <path
            d="M14 10H2v2h12v-2zm0-4H2v2h12V6zm4
            8v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zM2 16h8v-2H2v2z" />
        </svg>
      {:else if action === 'remove'}
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24">
          <path
            d="M6 19c0 1.1.9 2 2 2h8c1.1 0 2-.9 2-2V7H6v12zM19 4h-3.5l-1-1h-5l-1
            1H5v2h14V4z" />
          <path d="M0 0h24v24H0z" fill="none" />
        </svg>
      {/if}
    </div>
  {/if}
  <div class="video-snippet-thumb thumb-{thumbnailSize}">
    {#if hovered}
      <div class="playlist-play-indicator">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24">
          <path d="M8 5v14l11-7z" />
          <path d="M0 0h24v24H0z" fill="none" />
        </svg>
      </div>
    {/if}
    <img src={thumb.url} alt={title} />
  </div>
  <div class="video-snippet-meta">
    <h5>{title}</h5>
    <p>{channelTitle}</p>
  </div>
</div>
