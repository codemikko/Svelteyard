I get alot of people from the Svelte Community on Reddit asking about my method for using Lanyard, getting my Discord Presence and having it displayed on my website. Well, lucky for you, Im in the business where I share alot of my opensource scripts. 

I want to add that I will not be the first to claim this because not that I am just now learning svelte framework, I felt if Svelte was part HTML, how in Sam Hell it would be hard to do this. I searched for 3 days straight and could not get a straight answer which is what led to this point here. Once I slightly did a bunch of testing just to find out, theres only a few steps to do this. 

## First Step
You need to know and understand what Lanyard is and what it does. So Im going to school you a bit but this is from their Github themselves, so shouts out to [@Phineas](https://github.com/Phineas/) for this:

![Lanyard Official Logo - by Phineas](https://camo.githubusercontent.com/9e82c05e7ff58bb4bf4b9c8be87398d32a287c6f65dc742d2a1ea608dd78d676/68747470733a2f2f73746f726167652e676f6f676c65617069732e636f6d2f6c616e796172642f7374617469632f6c616e7961726474656d706c6f676f2e706e67)


## [](https://github.com/Phineas/lanyard#%EF%B8%8F-expose-your-discord-presence-and-activities-to-a-restful-api-and-websocket-in-less-than-10-seconds)🏷️ Expose your Discord presence and activities to a RESTful API and WebSocket in less than 10 seconds

Lanyard is a service that makes it super easy to export your live Discord presence to an API endpoint (`api.lanyard.rest/v1/users/:your_id`) and to a WebSocket (see below) for you to use wherever you want - for example, I use this to display what I'm listening to on Spotify on my personal website. It also acts as a globally-accessible KV store which you can update from the Lanyard Discord bot or from the Lanyard API.

You can use Lanyard's API without deploying anything yourself - but if you want to self host it, you have the option to, though it'll require a tiny bit of configuration.

## [](https://github.com/Phineas/lanyard#get-started-in--10-seconds)Second Step: Get started in < 10 seconds

- Go to [Discord](https://discord.gg) and download the Desktop client
- (optional) If don't own a PC, Laptop, Tablet, MacBook, you trippin, its 2023 bruh. "[Walmart](https://www.walmart.com/ip/Lenovo-Ideapad-3i-14-FHD-Laptop-Intel-Core-i3-1115G4-4GB-128GB-SSD-Windows-11-in-S-Mode-Platinum-Grey-81X700FGUS/125496557?athbdg=L1102)!" To the very least right? And no, really its not funny. The last couple of years has been extremely hard on a ton of people including young devs like all of us!

- To get your Discord ID you need to head to the user settings and turn on `Developer Mode`
![enter image description here](https://i.imgur.com/fyF1qV4.png)
Then goto the last message you sent somewhere on Discord, or send yourself a message we dont care... Once you do, right click on your name and `Copy ID`
![enter image description here](https://i.imgur.com/xTCnZxf.png)
- Make sure you have a [Spotify](https://spotify.com) account which is free and yusss, FWEE!
- If you already have Spotify, or even if once you create an account and logged in the Desktop Version of Spotify, you cannot be in Private Mode. If you are, Lanyard will act as if your really not  streaming music. Why should it? You wanna be private, then have at it! I suggest you not and join the rest of us who can care less what people think of our music, music is like.... coding. And coding is like... poetry!!
- Once all of that is done, on your discord once again:
![Oh, thats how!](https://i.imgur.com/pPhr6NS.png)

- Once your done with all that 10 seconds of work there, lol 

> Just [join this Discord server](https://discord.gg/UrXF2cfJ7F) and
> your presence will start showing up when you `GET
> api.lanyard.rest/v1/users/:your_id`. It's that easy. - Phineas

In your REST for Lanyard, all you need is lanyard's restful link and your Discord ID. So your Discord ID should look something like this: `https://api.lanyard.rest/v1/users/625796542456004639`
																This as an example is the ID that you would need			⤴
Remember `api.lanyard.rest/v1/users/:your_id`? Yup.

Ok to the last part which is the fun part!

## Sveltekit, HTML & Tailwind

Codes very simple guys, just copy this if your running svelte/sveltekit, its so simple, youll see:
This would go at the top of your example.svelte page where your going to add the HTML bit of it:

    
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

And add the rest of the code where you would want your roses to grow and smell good just like the image below:
![enter image description here](https://i.imgur.com/YDDeJJ6.png)
![enter image description here](https://i.imgur.com/NL3FSxk.png)

**Prettier 2.8.4**
[Playground link](https://prettier.io/playground/#N4Igxg9gdgLgprEAuEAdG7M2AYgJYBmABAK4DOcATgAqVwVRhwB0AJgIYzsC+qUWA7PmLkqtegiZtO7ZuzAw8ANzgB9aKtZ4ykSq030A1jAgAHIgB8LpCjToMpHLnIXK1GrToh7VAWwgARngANnC8-BiRgriENmL2kixOssHa8FB4UADmqiaqZKYQigQAnuGCFdgAAgAWML7BRKxwkM0AqgBKAJIAFKJ2EoxJMswFRYQlzDCU8oaqJJTBAJTlUWtYADxaSkRgwexkZAC8qCAEoQAeRHjwvmQAtEywVEQF8nD3F-cATESUECQoM1WPdfKwiPALjB7gB3Go3OBEXzQgAsr1M73uJXuADZTgA+PiVSobcx7A7HU6Q6FkXxEAjQaEBCDBcHU+5ZGbYnEABh5RACwQgYEMBK6AHI6QAZNIITJZCEQCBIDYAelMhIixLWGwClCIqs12sEGzwviyRPWVqwZEoYCOwH64gcw2cY2Kk3YwQCJF8qnYlBg80Wq2Nay9MBOIAAgt7fUQAxg0FrrVbyYcozV7gBGNEwnNo-6A4H3AgkYKNc5wL5kGqUTKGe780EBe4AVlOlrD6ENRJTYa2yl2+wzVOrNLpoXYWmy90UWTqBK7qZN7GX3cidbgBEzMBgpjISFVqrMCFGhQ9zEgvlV01mqsdtmdiWkbovEymMxFqjwrF4yY3QQuEoLI4EjU5VEFdgoFFACV1TOhgijKBvG3Kg6EoTt+3g9AjRwq0NjeKBhwpKNBWFQwIXHDk6AQds+QJR94kGRwRndD8yGgLJuDVIi8MAyJCIxYj00pEByJFKioU+MhGKdBIhlfWR2NKORAzSHjVT49d8LVdh+Pw9BSRI0cQHZC5ZJAfEmIGF0lPPcZVK9H1fE0jUdO1NVtgMzzVW8jy1mAJA4GCChQ3gwcdlEqMqyuBE7keBB4H1N4mE+H4-gBIE4BBMEpOhTk4Dots+SRVF0Uxbkl2wzzthMsSszbIh8yaotspBMsK3pS57lresYKbAUshooqoHo5tfFbDsQANHyIrqxQYFCKMAHkoFSKA4FOeqo3ZWkiCnGdhvnOoIUoQEwE4LarLWja4C85Q5pJPzHoCrBgFVQhwuiYLQrCN7BLq6LTli65bgeJ5koqtKvl+NqSzy9lNpIO9gnGsr7jRVKPiqqyAc2IGRwa9tmpJ+GctLctKx6vqG0GgJhs5dhsRK-kOEoQwkAZmjmZ+UqW3bbbexqgiFpuZbThWghzkya6drHaT9sO+U5zwBcYDOi6roJKWZc2h6lCenUXsNgGPq+gLzYIcIQAAGhAMxFGgMhkFAAN-hhagAwQF2UC9GFmZd+29VmMCAGV2F8OAZU25ACC9Chg6-Qxw4xMB5WQaYSDge24EmnLgSlaCshIdhQIAMW8XxOEUbJkBAdgUYgO2QDqBoAHV4XgbGw59m5lBuEp67AQ4W8yWwYFoMvq7jhOc5AAArMgLjD+VQgARRIIo4Fnv77YxShbHrtvghb0x+pgdvfxgGpkAADh5ff-goduZlMevz4kSgVBbgBHLf4C0DML7BuDxNo5Ryi3Og-88B0CnlkGeSB457xABQXweBM7nXnmQNecBox7nrD6QBVAY47yQXPe2ODsgbwAWQ5BiczLsACFfVgN9kDfHtneEI8oADCEBfCIJAPQNsLdRAABUmG+3ofPJQ2cujZVgGHMA9ZTAwGjECMOMASihF3mFbgQA)
<!-- prettier-ignore -->


**Input:**
<!-- prettier-ignore -->
```html
				<h1>Welcome to your new SvelteKit app</h1>
{#if userPresence.data}
  {userPresence.data.discord_status}
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

```



There you go mah bowah! Pat-yourself on the back, youve did it! Now go below and I will leave you an example of mine other than the pics there, but to show you what your API will look like and you can use it as a referance in-case you want to customize this yourself. 

Mine is mixed with [Tailwind CSS](https://tailwindcss.com/) as well just so you'd know.

**Prettier 2.8.4**
[Playground link](https://prettier.io/playground/#N4Igxg9gdgLgprEAuEwA6IDOBXMY6aYZIwBO2cANBgCYCGMdx6WADhDAJYBmAnsxjJ0wAawD6nGsQwAWACoBOKAEkAahACOAdQCsADUxQA5nQikANgEYAVpbkZqILgFsCjZ6yLIWmRqRjElgBsAOwAHDoADADM0TLRliHRjghSyMHhUXEhlgkKAL6OmNBG0iAAoqScmAAEAIJQNDUAIryGEM6cTCCOdP7VAcgYdTUAyiK85pwwLVUAbnAOGHTmAEbYzmJ9MGLYFmUAFjAwnkgA9GecAHSYYDRQV5CXznRGcGd0q6FBwTSR-5FVgAmJJgMIyODcOCRMDCHRQ6JhbhhMJBaKrGhgHRLEArdbOMpaTikAhXMkNJpyA5wGoAYWg+FYMwA8twagAhEkMA6cYwYQoYKa+BC8oxiGAQMSYdhcPjEMgURwiObMAUgGjVSCkGi7TBwUgCEDYPWkKB0VxlACynBEIggONY2FWUzAYm45leXiQQRkjkkZSCQJ0IQUQR0MiBMh0QX+MjRChxGulHt4YjNFuQUGw5nMjiTYCqnTNEoNQxAuWiIRxqw4xG4Kz1vTmDD6YhocC1DE40GIWZzTZbpaQGEsljAQTCljCcDgCmi3HoQRolZ0CjgOg3+DoQToQLAq35ec1Zh1vgYxrK0CmUEWPWWYC4c2mnAIxAA2iwYLxWLekEDHC4bjmp4hpnv4gShBEMRxAkSQpI0EGZDEMg5HkaqYLwUCuv6ZbyEoaiaLoBjGKYFg2HYOJnvAZQjOMkzTLMnALJRBCYN2UASGkw4gJEe54NEaL7iEYDcLkq4hHANAhHQkSWNwgKrHQm4Ots-DeBgOHcdKHA8Lw3pBiGYYRlGMaRHG0QJiAarpr+GCjDKuk4ppGDabKemWDi7qesQMhhHmcCMJw5hehglTVPUjQtG0UAdF0OIFnADCSVsgxIBkUGxDIKECb0hABV6LAeqQbzinAAAeqUYESJKYGSVwUjUVI0vSWFwEyNSshyXIwDyfJ3iARUlZwLxvGUrm6UgnzfL8AKAiC0RghCUIwnCCJIiiaIYli-L5AAur0D5MXAYjQG2x7amIADucAHsg9bBVQ96Psdp35ieYjOBAqxBb+92Nk9R0nRxb0Xe2mAiBKrDyuQcD5PkPQgBATLsZg3i4qQpAQJdAAKfQIKjKArJddBtAjqykMIIgBaM5pwAAMrycB3Q2VAgOTlPU6wwiisgCqs3AzirJJ7Y0HTdDGNgrxwAAYmYLzHDzhPYBKCNHM45haDy8DSsIcCjPjz5Pl+yDgIQCO8iaMDYxTRgvMzD2UCA1iYGVoyiuYcAAIrYBwTNIH9rNc6QJom870A6AjrBVLARI0D1yBhJEjtRxAepaBTrAm1HBD6gsCMaD78DW0jBO4pgAC0N6SZJCMkgXxJwNbrx2-7LOO3qnS8zD7fu3AdTHFU6xF-qDM3vbjZYL33u++PrOMKssfx3+jtCEFor0s4LcgAQEeO8acByJ8BMB47cwUMojQIDAowFpwTIUqMX4e7PcNAA)
<!-- prettier-ignore -->
```sh
--parser json5
```

**Input:**
<!-- prettier-ignore -->
```json5
{"success":true,"data":{"spotify":{"track_id":"4T9nIVoqW5Xsngaorl1j1T","timestamps":{"start":1678503343173,"end":1678503471319},"song":"Eris And Dysnomia","artist":"A Skylit Drive","album_art_url":"https://i.scdn.co/image/ab67616d0000b273c84efe0cac5fe38f8863bdc5","album":"Wires...And The Concept Of Breathing"},"listening_to_spotify":true,"kv":{},"discord_user":{"username":"Mikko","public_flags":64,"id":"625796542456004639","display_name":null,"discriminator":"1337","bot":false,"avatar_decoration":null,"avatar":"11c6818ee93fda6d3759e555cea6a2cb"},"discord_status":"online","activities":[{"type":2,"timestamps":{"start":1678503343173,"end":1678503471319},"sync_id":"4T9nIVoqW5Xsngaorl1j1T","state":"A Skylit Drive","session_id":"02ccc363cb7cf13597ed7a01f0bba55c","party":{"id":"spotify:625796542456004639"},"name":"Spotify","id":"spotify:1","flags":48,"details":"Eris And Dysnomia","created_at":1678503344736,"assets":{"large_text":"Wires...And The Concept Of Breathing","large_image":"spotify:ab67616d0000b273c84efe0cac5fe38f8863bdc5"}}],"active_on_discord_web":false,"active_on_discord_mobile":false,"active_on_discord_desktop":true}}
```

**Output:**
<!-- prettier-ignore -->
```json5
{
  success: true,
  data: {
    spotify: {
      track_id: "4T9nIVoqW5Xsngaorl1j1T",
      timestamps: { start: 1678503343173, end: 1678503471319 },
      song: "Eris And Dysnomia",
      artist: "A Skylit Drive",
      album_art_url: "https://i.scdn.co/image/ab67616d0000b273c84efe0cac5fe38f8863bdc5",
      album: "Wires...And The Concept Of Breathing",
    },
    listening_to_spotify: true,
    kv: {},
    discord_user: {
      username: "Mikko",
      public_flags: 64,
      id: "625796542456004639",
      display_name: null,
      discriminator: "1337",
      bot: false,
      avatar_decoration: null,
      avatar: "11c6818ee93fda6d3759e555cea6a2cb",
    },
    discord_status: "online",
    activities: [
      {
        type: 2,
        timestamps: { start: 1678503343173, end: 1678503471319 },
        sync_id: "4T9nIVoqW5Xsngaorl1j1T",
        state: "A Skylit Drive",
        session_id: "02ccc363cb7cf13597ed7a01f0bba55c",
        party: { id: "spotify:625796542456004639" },
        name: "Spotify",
        id: "spotify:1",
        flags: 48,
        details: "Eris And Dysnomia",
        created_at: 1678503344736,
        assets: {
          large_text: "Wires...And The Concept Of Breathing",
          large_image: "spotify:ab67616d0000b273c84efe0cac5fe38f8863bdc5",
        },
      },
    ],
    active_on_discord_web: false,
    active_on_discord_mobile: false,
    active_on_discord_desktop: true,
  },
}

```

Questions on my methods... well, post an issue or just re-read from the beginning to retrace your steps. Ay, Im just telling you how I did it and it worked. I had no one to help me so... There were alot of references and ideas. But I also want to thank you for at least reading this far and I hope you enjoy your Presence!

## Contribute

If you have another way or would like to add your own way to this repo and share with us all, please feel free to:
[Fork](https://github.com/codemikko/Svelteyard/fork) a copy
Create a **new branch** for this commit and start a pull request.  [Learn more about pull requests.](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)


> 
> Written in [Markdown](https://code.visualstudio.com/docs/languages/markdown) by [CodeMikko](https://github.com/codemikko) using [vsCode](https://code.visualstudio.com)
