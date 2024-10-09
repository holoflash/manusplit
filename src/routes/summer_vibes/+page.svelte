<script lang="ts">
    import Lyrics from "$lib/Lyrics.svelte";

    const lyricsText = `
# Summer Vibes
## Warmup
### Cruel Summer
A while ago, I heard the sound of children's laughter 
Now it's quiet, so I guess they left the park
This wooden bench is getting harder by the hour
The sun is going down, it's getting dark
***
I realize I'm cold, the rain begins to pour
As I watch the windows on the second floor
The lights are on, it's time to go (ooh)
It's time at last, to let him know
***
I believe it would be fair to say, "You look bewildered"
And you wonder why I'm here today
And so you should, I would
When I left, I felt I've had enough
But in the shape and form, I appear now
I have learned to cope, and love and hope is why I am here now
***
And now you see another me, I've been reloaded
Yeah I'm fired up, don't shut me down
I'm like a dream within a dream that's been decoded
I'm fired up, I'm hot, don't shut me down
***
I'm not the one you knew, I'm now, and then combined
And I'm asking you to have an open mind (and I won't be the same)
I'm not the same this time around (ooh)
I'm fired up, don't shut me down
`;
    const storageKey = "summer_vibes";
</script>

<div>
    <Lyrics {lyricsText} {storageKey} />
</div>
