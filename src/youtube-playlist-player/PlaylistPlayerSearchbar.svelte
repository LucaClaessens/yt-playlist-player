<script>
  import { createEventDispatcher } from "svelte";
  import { ytApiKey, searchResults } from "./stores.js";

  let searchQuery = "";

  function handleKeyDown(event) {
    if (event.code === "Enter") {
      return searchYoutubeVideos(searchQuery);
    }
  }

  function handleClick(event) {
    return searchYoutubeVideos(searchQuery);
  }

  async function searchYoutubeVideos(query) {
    if (query.length === 0) {
      return void 0;
    }

    const res = await fetch(
      `https://www.googleapis.com/youtube/v3/search?part=snippet&order=relevance&q=${query}&type=video&key=${$ytApiKey}`
    );
    const results = await res.json().then(response => response.items);
    searchResults.update(r => results);
  }
</script>

<style>
  .searchbar-wrapper {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    width: 100%;
    padding: 15px 0;
  }
  input {
    margin: 0;
    border-radius: 2px 0 0 2px;
    border-right: 0px;
    width: calc(100% - 64px);
    max-width: 400px;
  }
  button {
    cursor: pointer;
    width: 65px;
    border-radius: 0 2px 2px 0;
    margin: 0;
    padding: 2px 0;
  }
  button:hover {
    background-color: #dedede;
  }
  button svg {
    width: 24px;
    height: 24px;
    transform: translateY(2px);
    fill: #5a5a5a;
  }
</style>

<div class="searchbar-wrapper">
  <input
    type="text"
    placeholder="Zoeken"
    bind:value={searchQuery}
    on:keydown={handleKeyDown}
    aria-label="Search input" />
  <button on:click={handleClick} aria-label="Search button" title="Search">
    <svg
      viewBox="0 0 24 24"
      preserveAspectRatio="xMidYMid meet"
      focusable="false"
      style="pointer-events: none;">
      <g>
        <path
          d="M15.5 14h-.79l-.28-.27C15.41 12.59 16 11.11 16 9.5 16 5.91 13.09 3
          9.5 3S3 5.91 3 9.5 5.91 16 9.5 16c1.61 0 3.09-.59
          4.23-1.57l.27.28v.79l5 4.99L20.49 19l-4.99-5zm-6 0C7.01 14 5 11.99 5
          9.5S7.01 5 9.5 5 14 7.01 14 9.5 11.99 14 9.5 14z" />
      </g>
    </svg>
  </button>
</div>
<div class="player" />
