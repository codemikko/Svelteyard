<script>
  import { onMount } from 'svelte';

  let userPresence = {};

  async function fetchPresence() {
    const res = await fetch('https://api.lanyard.rest/v1/users/625796542456004639');
    if (res.ok) {
      userPresence = await res.json();
      console.log(userPresence); // add this line for debugging
    }
  }

  onMount(() => {
    fetchPresence();
    setInterval(fetchPresence, 10000);
  });

  $: currentActivity = userPresence?.data?.spotify?.timestamps?.start;
  $: currentSong = userPresence?.data?.spotify?.song ?? "";
</script>

<h1>Welcome to your new SvelteKit app</h1>
{#if userPresence.data}
<!--   {userPresence.data.discord_status} -->
  {#if userPresence.data.active_on_discord_desktop || userPresence.data.active_on_discord_mobile}
    {#if userPresence.data.listening_to_spotify}
      {@html decodeURI(userPresence.data.spotify.track_url)}

      <div class="flex items-center space-x-2 rounded-md text-white mt-4 space-y-6">
        <p class="text-sm font-bold text-gray-600 block">I'm Listening too:</p>
        <br />
        <img
          src={userPresence.data.spotify.album_art_url}
          alt="Album art"
          class="h-10 w-10"
        />

        <div class="text-sm leading-tight">
          <a
            href="https://open.spotify.com/track/{userPresence.data.spotify.track_id}"
            target="_blank"
            rel="noreferrer"
          >
            <span class="block text-green-500">{currentSong}</span>
            <span class="block text-xs">{userPresence.data.spotify.artist}</span>
          </a>
          <p class="text-xs">{userPresence.data.spotify.album}</p>
        </div>
      </div>
    {:else}
      <div class="flex items-center space-x-2 rounded-md text-green-500 mt-4 space-y-6">
        <div class="h-2 w-2 bg-green-500 rounded-full inline-block ml-1" />
        <div title="Online" class="text-sm leading-tight font-bold truncate">🟢 Online</div>
      </div>
    {/if}
  {:else}
    <div class="flex items-center space-x-2 rounded-md text-green-500 mt-4 space-y-6">
      <div class="h-2 w-2 bg-green-500 rounded-full inline-block ml-1" />
      <div title="Online" class="text-sm leading-tight font-bold truncate">🟢 Online</div>
    </div>
  {/if}
{:else}
  <div class="flex items-center space-x-2 rounded-md text-neutral-500 mt-4 space-y-6">
    <div class="h-5 w-5 rounded-full flex-shrink-0 bg-gray-500 dark:bg-gray-200 -mb-5" />
    <div title="Offline" class="text-sm leading-tight truncate">🔴 Offline</div>
  </div>
{/if}
