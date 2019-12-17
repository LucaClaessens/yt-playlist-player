<script context="module">
  var tag = document.createElement("script");

  tag.src = "https://www.youtube.com/iframe_api";
  var firstScriptTag = document.getElementsByTagName("script")[0];
  firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);

  window.onYouTubeIframeAPIReady = function() {
    window.dispatchEvent(new Event("youTubeIframeAPIReady"));
  };
</script>

<script>
  import { currentVideoId } from "./stores.js";
  import { createEventDispatcher } from "svelte";

  let player;

  let playerId =
    "player-" +
    Math.random()
      .toString(36)
      .substring(2, 11);

  const dispatch = createEventDispatcher();

  window.addEventListener("youTubeIframeAPIReady", function() {
    player = new YT.Player(playerId, {
      height: "338",
      width: "600",
      $currentVideoId,
      events: {
        onReady: onPlayerReady,
        onStateChange: onPlayerStateChange
      }
    });
  });

  function onPlayerReady(event) {
    event.target.playVideo();

    currentVideoId.subscribe(id => {
      player.loadVideoById({ videoId: id });
    });
  }

  function onPlayerStateChange(event) {
    dispatch("stateChange", {
      data: event.data
    });
  }
</script>

<style>
  #player {
    width: 100%;
    height: 450px;
    background-color: black;
  }
</style>

<div id={playerId} />
