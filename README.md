# hide-and-seek

A single-page browser toy: a shy cartoon girl who sits on a park bench. Move your cursor near her and she runs away; let her be and she walks back to the bench and sits down. Catch her by clicking to unlock different modes. Everything — character, scenery, weather, day/night cycle — is drawn from scratch with the HTML5 Canvas 2D API.

Live as plain HTML — no build step, no dependencies.

## Run it

Just open `index.html` in any modern browser:

```bash
xdg-open index.html   # Linux
open index.html       # macOS
start index.html      # Windows
```

Or serve the directory with any static server (`python3 -m http.server`, `npx serve`, etc.).

## What's in the scene

### The character
- Front-facing cartoon girl drawn with bezier curves: head, hair, eyes, mouth, body, arms, legs, shoes
- Mature anime restyle: long dark hair with a center-parted side sweep, almond eyes with a winged outer-corner liner, thin arched eyebrows, soft cupid's-bow lips
- Daily wardrobe: a new dress color and outfit silhouette every in-game day
  - **7 colors**: pink, sunflower, mint, lavender, sky, coral, tangerine
  - **8 silhouettes**: bell, gown, sundress, tee+skirt, overalls, shorts+tank, polka-dot, sailor
  - Color and outfit are rolled independently with `Math.random()` at midnight, so 56 combinations come up before any exact repeat
- Sits with knees together when seated, walks with a subtle swing, runs with arms up when scared

### The bench
- Wooden park bench wide enough for two: slatted backrest, armrests, cross-brace, plank-seam on the seat
- The character starts seated, walks back to it when not chased, and her hip lines up exactly with the seat top

### The ground
- ~200 grass blades that sway gently with time
- ~25 flowers in five colors with stem, five petals, and yellow center
- All ground decoration tints darken at night

### The sky (90-second day cycle)
- Sky gradient interpolated across midnight, dawn, sunrise, noon, afternoon, sunset, dusk
- Sun arcs east → south → west during the day
- Moon arcs across the sky at night and **cycles through 8 phases** over 8 days: new → waxing crescent → first quarter → waxing gibbous → full → waning gibbous → last quarter → waning crescent
- ~80 stars fade in with the darkness and twinkle individually
- Clouds drift across the sky at independent speeds; size and tint react to weather

## Controls

Two interactions:

| Action | Effect |
|---|---|
| Move cursor near her | She stands up, flees horizontally on the ground (or in 2D in angel mode, or teleports in vampire mode) |
| Click on her body | First click evolves her into **angel mode** (during the day) or **vampire mode** (at night). Subsequent hits show a floating "Gotcha!" / "Found you!" / "Boop!" / etc. message |

The clock in the top-right shows the current in-game time. The weather selector at the top-center has six buttons:

- ☀️ **Sunny** — sun + scattered white clouds
- ⛅ **Cloudy** — denser clouds, pale tint, sun dimmer
- 🌧️ **Rainy** — angled rain, dark sky, rain clouds, puddles form on the ground, she carries an umbrella
- ⛈️ **Stormy** — heavier rain + random zigzag lightning bolts with glow halo and flicker
- ❄️ **Snowy** — snowflakes drifting on a sine wave, snow accumulates on the ground and on the bench
- 🌫️ **Foggy** — vertical fog gradient overlay, sun/moon hidden

## Modes

### Normal (default)
Ground-bound. Walks to the bench, sits, flees horizontally when threatened, returns to the bench when safe.

### Angel mode — click during the day
- Two big white feathered wings sprout, with a subtle flap animation
- She can move freely in 2D (chase her anywhere on the screen)
- ~1.5× faster than normal mode
- Bounces off all four edges (top, left, right, sky-floor above the ground)
- In rainy/stormy weather her wings get wet (drawn drooping in grey), she can't fly and reverts to grounded behavior
- Ends at sunset: she falls under gravity to the ground (wings stay visible until she lands) and mode returns to normal

### Vampire mode — click at night
- Dark bat-style wings with five finger tips, concave membrane scallops, glossy highlight, and a pointed thumb claw
- Instead of flying continuously, **she teleports** to a random safe spot when threatened, with a 450 ms cooldown and dark purple smoke poofs at the old and new positions
- Hovers in place when not chased
- Skips the portable umbrella regardless of weather
- Ends at sunrise: she teleports straight to the bench and sits down, then mode returns to normal

## Weather accumulation
- **Snow** builds up over ~20 s into a wavy white blanket along the horizon, with snow caps on the bench seat, backrest, and armrests
- **Rain** spawns dark puddles randomly across the ground, accumulating while it rains and fading out when the rain stops
- **Lightning** in storms: a freshly-generated zigzag bolt with 1-3 forks renders inside a quick attack + slow decay envelope with sine flicker

## Project layout
- `index.html` — everything is in this single file: HTML scaffolding, CSS for the on-screen UI, and one big `<script>` block with the canvas rendering loop

## Tech notes
- Single `<canvas>` filling the viewport, redrawn every animation frame
- No external libraries
- Movement, rendering, and state machine all live in one closure inside the script tag
- Day cycle uses `performance.now()` time; outfits use `Math.random()` rolled once per day index

## Credits
Code written collaboratively with [Claude Code](https://claude.com/claude-code).
