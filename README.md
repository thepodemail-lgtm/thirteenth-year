# The Thirteenth Year - Stage 1

Play: https://thepodemail-lgtm.github.io/thirteenth-year/ (tap **PLAY FULLSCREEN** on a phone for landscape).

Plain HTML + JavaScript, no build step. Edit a file on github.com and GitHub Pages republishes in about a minute.

## Where things live

| File | What's in it |
|---|---|
| **`js/tuning.js`** | **Start here.** Every tweakable number with plain-English notes: run speed, acceleration, jump / double jump, coyote and buffer times, sword reach and damage, arrows, hearts and lives, each enemy's HP and speed, hearts lost per hit, Deva duration, Vaka's HP, music and sound-effect volume. |
| `index.html` | Page shell: screen layout (side pads in landscape, pads below in portrait), PLAY FULLSCREEN + landscape lock, touch D-pad/buttons, toolbar, settings menu, hosted MP3 music player. Loads the scripts below in order. |
| `js/engine.js` | Canvas, shared helpers, game state, physics/collision, the main update loop (incl. enemy behaviour each frame) and drawing the world. |
| `js/audio.js` | Chiptune music sequencer, sound effects, hosted-MP3 music with chiptune fallback. |
| `js/sprites.js` | Pixel-art builder and every sprite: Vel, the dog, enemies, Vaka, items, icons. |
| `js/input.js` | Keyboard controls and button state. |
| `js/stage1.js` | The Stage 1 map, background art, enemy placement. |
| `js/player.js` | Vel and the dog: sword/spin attacks, getting hurt, falling, dying, pickups, dog AI. |
| `js/enemies.js` | Enemy spawning (size and HP) and damage to enemies. |
| `js/boss.js` | Vaka, the boss: intro, attacks, phases, damage. |
| `js/ui.js` | Text, HUD, title screen, win/lose cards, settings toggles (invincibility, unlimited arrows). |
| `js/main.js` | Boot: builds level and art, starts the game loop. |

All game scripts share one global scope, so load order in `index.html` matters.

Music: `victory-at-the-peak.mp3` and `last-stand-at-the-spire.mp3` from the `thirteenth-year-music` repo via jsDelivr; falls back to built-in chiptune if they can't load.
