# The Thirteenth Year: Vibe-Coding Playbook

**Who this is for:** Daniel (no coding needed) and Meta's Muse AI.
**Why it exists:** Game Dev (the AI agent that built this game) gets changes right; Muse keeps breaking things. This document writes down exactly what Game Dev does, so Muse can copy it.

Everything below comes from this repo: `REBUILD.md`, `SAVING-TOKENS.md`, `README.md`, `js/tuning.js`, `index.html` and the git history. When in doubt, the code wins.

---

## 1. Where everything lives

| What | Where |
|---|---|
| **Play the game (live)** | https://thepodemail-lgtm.github.io/thirteenth-year/ |
| **Game code + art** | https://github.com/thepodemail-lgtm/thirteenth-year |
| **Music** | https://github.com/thepodemail-lgtm/thirteenth-year-music |
| **Exact rebuild spec** | `REBUILD.md` in the game repo. This is the source of truth for rebuilding. |
| **Knob file (numbers you can change)** | `js/tuning.js` |
| **Working-lean guide + locked rules** | `SAVING-TOKENS.md` |

### Inside the game repo
- `index.html`: the page shell (layout, touch buttons, fullscreen button, settings menu, music player). Loads all the scripts.
- `js/`: the game, split into 13 small files, one job each (list in section 2).
- `assets/art/`: the finished art: `vel.png`, `dog.png`, `boar.png`, `hawk.png`, `vaka.png`, `vaka_parts.png`, `title.png`, `far.png`, `ruins.png`, `arena.png`, `tiles.png`.
- `assets/art/frames.json`: where each animation frame sits on each sheet (`[x, y, w, h]`), plus Vaka's 12-part rig (`vakaRig`).
- `assets/art/src/`: Daniel's original drawings (`vel_strip.jpg`, `dog_strip.jpg`, `boar_hawk.jpg`, `boss_hall.jpg`, `jungle_far.jpg`, `ruins.jpg`, `keyart.jpg`).
- `assets/art/tools/`: the Python (Pillow) scripts that turned the drawings into the PNGs: `build.py`, `sprites.py`, `bg.py`, and `anim.py` (makes the in-between animation frames and cuts Vaka into parts; run it once on a clean checkout because it writes back into the sheets).

### Music (in the music repo)
- **Stage:** `stage1-v2.mp3`
- **Boss:** `last-stand-at-the-spire.mp3`
- Streamed through jsDelivr: `https://cdn.jsdelivr.net/gh/thepodemail-lgtm/thirteenth-year-music@main/<file>`
- Deva mode uses the built-in chiptune theme (the music switch, like Mario's star).
- Note: `victory-at-the-peak.mp3` is still in the music repo and named in the README, but the code no longer uses it.

### How to get a copy
- **ZIP (no account tricks):** open the repo page, tap the green **Code** button, then **Download ZIP**. Do the same on the music repo.
- **Clone (for a computer with git):**
  `git clone https://github.com/thepodemail-lgtm/thirteenth-year.git`
  `git clone https://github.com/thepodemail-lgtm/thirteenth-year-music.git`

---

## 2. How to rebuild it exactly

### Fastest way (identical game, zero code)
1. On GitHub, open the game repo and tap **Fork** (or download the ZIP and upload the files to a new repo).
2. In the new repo: **Settings → Pages → Source: Deploy from a branch → `main` / root → Save**.
3. Wait about a minute. The game is live at `https://<your-username>.github.io/thirteenth-year/`.

There is **no build step**. It is plain HTML + JavaScript. The music keeps streaming from the original music repo, so nothing else is needed.

### The long way (rebuilding from scratch)
Follow `REBUILD.md` section 18 ("Rebuild order") step by step, and check off section 19 ("Acceptance checklist") at the end. Do not skip ahead; each step is tested before the next.

### Exact file list and load order
All scripts share one global scope, so **the order matters**. From `index.html`:

| # | File | Job |
|---|---|---|
| 1 | `js/tuning.js` | Every tweakable number |
| 2 | `js/engine.js` | Canvas, helpers, game state, physics, main loop, drawing the world |
| 3 | `js/audio.js` | Chiptune music, sound effects, MP3 music with chiptune fallback |
| 4 | `js/sprites.js` | Procedural pixel-art for every sprite |
| 5 | `js/input.js` | Keyboard controls and button state |
| 6 | `js/stage1.js` | Stage 1 map, backgrounds, enemy placement |
| 7 | `js/player.js` | Vel and the dog: attacks, hurt, falling, dying, pickups, dog AI |
| 8 | `js/enemies.js` | Enemy spawning and damage |
| 9 | `js/boss.js` | Vaka: intro, attacks, phases |
| 10 | `js/ui.js` | Text, HUD, title, win/lose cards, settings |
| 11 | `js/art.js` | Loads the hand-drawn art pack; falls back to procedural art if a file fails |
| 12 | `js/anim.js` | Tweened frames, Vaka's skeletal rig, sound overrides (princely sword, arrow thunk) |
| 13 | `js/main.js` | Boot: builds the level and art, starts the loop |

Then an inline script in `index.html` handles layout, touch controls, fullscreen + landscape lock, and the MP3 music hook.

---

## 3. How Daniel talks to Game Dev (the pattern that works)

1. **One batch of feedback per pass.** Play, write down everything, send it in one message. Ten notes cost about the same as one. Game Dev makes one edit pass per batch.
2. **Say what you want and why, in plain words.** Example from the history: "majestic sword sound since he's a prince" became the "princely sword" in commit `5ca53ca`. The *why* lets the AI choose the right fix.
3. **Feedback format** (from SAVING-TOKENS.md): **Where** ("second pit after the lanterns"), **What happened** ("I jumped early but still fell"), **What you expected** ("I should have cleared it").
4. **Screenshots and reference images** when the *look* is the problem. Name references (Matendoji, Batman: Return of the Joker).
5. **Give finished art.** Daniel's drawings went in as `assets/art/src/*.jpg` and became the art pack. Plain white background, one pose per frame, side by side cuts out cleanest.
6. **A thumbs-up is a full yes.** Short answers are fine.
7. **Approve a one-line plan before big features.**

### Locked rules (never re-ask, never break)
- Vel uses a **sword, never a staff**.
- The **beam fires only at full health**.
- The **second jump is a spinning sword attack**.
- **Deva mode switches the music** (like Mario's star).
- **Enemies come from Indian mythology** (Yaksha, Vetala, Rakshasa, cobra, imps). Names: Vel (Kathirvelan), Sanjaya narrates, the Dog, Vaka the rakshasa.
- **Light, receding backgrounds; sprites pop. No scanlines.**
- **Reference: Matendoji** (Famicom) over the US Crystal Palace; Batman: Return of the Joker for art quality; Strider, Rocket Knight Adventures, Ogre Battle for inspiration.
- **Never rip NES art or music.**
- **$0 budget.** Free first; flag any cost before spending.
- **Never hand Daniel code to run.** The AI does the work; Daniel only edits numbers in `tuning.js` if he wants to.
- Open decision that is **Daniel's to make**: Hunter/Shiva duel vs Vaka as the Stage 1 boss (code currently ships Vaka).

---

## 4. How Game Dev implements (what it does right)

You can see the method in the git history. Each commit is one clear step:

| Commit | What it shows |
|---|---|
| `282d295` The Thirteenth Year - Stage 1 (phone fullscreen build) | First working build, built for phones |
| `b2bdcf6` Split into multi-file structure + js/tuning.js **(behavior identical)** | Restructured *without* changing how the game plays |
| `b998063` Use new Stage 1 music | One small change, one commit |
| `d463b28` Hand-drawn art pack ... **(procedural fallback)** | New art added, old drawing kept as a safety net |
| `d4178b9` Add token-saving guide | Docs |
| `5ca53ca` Smooth animation, skeletal Vaka rig, dog tuning, audio remix | One feedback batch = one pass, all listed in the message |
| `caae332` Add REBUILD.md | Full spec written from the code |

### The habits
1. **Pull the latest first.** Always start from what is live, never from an old copy.
2. **Read the existing code before changing anything.** Find the exact file and function that does the thing.
3. **Small files, one job each** (section 2). A change touches one or two files, not the whole game.
4. **All tweakable numbers live in `js/tuning.js`** with plain-English comments, so Daniel can change them himself. New knobs (e.g. the `dog` and `anim` blocks) get added there too.
5. **Small targeted edits, not rewrites.** Change the lines that need changing.
6. **Preserve everything not asked to change.** The split commit says "behavior identical"; the art pack keeps a procedural fallback; hitboxes never change when art changes.
7. **Locked rules as a checklist.** Checked before every change is called done.
8. **Back up before editing** so any mistake can be undone.
9. **Test in a real (headless) browser after each change:** load the game, take screenshots, check the console for errors.
10. **Honest reporting:** says what was actually verified versus only code-reviewed (e.g. REBUILD.md notes "the sounds haven't been tested by ear on a real device"), and says when heavy checks were skipped.
11. **New filename for changed music** (`stage1-v2.mp3`) so phones don't play a cached old copy.
12. **Clear commit message, push, then check the live site** after the ~1 minute Pages delay.
13. **One canvas, never remounted.** There is a single 320×180 `<canvas id="cv">`; layout just resizes around it.
14. **Fullscreen + landscape lock happen on a tap.** Browsers only allow it from a user gesture, so the **PLAY FULLSCREEN** button calls `requestFullscreen` and then `screen.orientation.lock('landscape')`.
15. **Audio unlocks on the first tap.** The sound system is created and resumed on the first press (`audioInit()` + `AU.ctx.resume()` in `start()`/`pressStart()`), because phones block sound until the player touches the screen.
16. **Asks only when the decision is truly Daniel's** (story/design choices like Hunter/Shiva vs Vaka). Everything technical, it just does.

### Why Muse fails, and the fix for each

| Failure | What goes wrong | Fix |
|---|---|---|
| Rewrites the whole game from scratch each time | New bugs everywhere; the game "forgets" how it felt | Edit only the files needed; give Muse the current file first |
| Loses earlier features | Double-jump spin, Deva music, dog etc. disappear | Paste the locked-rules list every session; ask "what did you keep?" |
| One giant file regenerated | Too long, gets cut off or mangled | Keep the 13-file split; one file per request |
| Invents functions/APIs that don't exist | Game crashes on load | Paste the real file; "use only functions that appear in the files I gave you" |
| Doesn't test | "Done!" but broken | Require the verification checklist (section 5) and check the live site yourself |
| Ignores locked rules (staff, scanlines, dark backgrounds) | Off-style game | Rules at the top of every session |
| Embeds the game in an iframe/preview pane | Can't go fullscreen or rotate to landscape | Only test on the real GitHub Pages link |
| Forgets the audio unlock | No sound on phones | Audio must start from the first tap |
| Moves numbers into code | Daniel can't tweak | Every number goes in `tuning.js` with a comment |
| Same music filename for a new track | Phone plays the old song | New track = new filename |

---

## 5. Copy-paste prompts for Muse

### A. Master project-context prompt (paste at the start of EVERY session)
```
You are editing an existing web game, "The Thirteenth Year" (Stage 1), an NES-style
2D action platformer. Repo: github.com/thepodemail-lgtm/thirteenth-year.
Live: https://thepodemail-lgtm.github.io/thirteenth-year/
Plain HTML + JavaScript, no build step, hosted on GitHub Pages.

ARCHITECTURE (one global scope; load order in index.html matters):
tuning.js, engine.js, audio.js, sprites.js, input.js, stage1.js, player.js,
enemies.js, boss.js, ui.js, art.js, anim.js, main.js. One 320x180 canvas (id "cv"),
never recreated. All tweakable numbers live in js/tuning.js with plain-English comments.

LOCKED RULES (never break):
- Vel uses a sword, never a staff. Beam only at full health. Double jump = spinning sword attack.
- Deva mode switches the music (like Mario's star).
- Enemies from Indian mythology. Vel, Sanjaya (narrator), the Dog, Vaka the rakshasa.
- Light receding backgrounds, sprites pop, NO scanlines.
- Reference Matendoji (Famicom). Never copy NES art or music.
- $0 budget. Fullscreen + landscape lock only from a tap. Audio unlocks on first tap.
- I am not a coder: never ask me to run code.

HOW TO WORK:
1. Use only the files I paste. Do not invent functions that are not in them.
2. Edit ONLY the files needed. Make the smallest change that works.
3. Return the FULL contents of each changed file only. Never rewrite or return
   untouched files. Keep everything I did not ask to change exactly as it is.
4. Any new number goes in tuning.js with a plain-English comment.
5. Before you answer, tell me in one line which files you will change and why.
6. End with the verification checklist.
```

### B. Feature change
```
FEATURE: [what I want, in plain words]
WHY: [the reason, e.g. "he's a prince, so the sword should sound majestic"]
WHERE IN THE GAME: [e.g. "the boss fight", "every jump"]
FILES (pasted below): [e.g. js/player.js, js/tuning.js]
Keep everything else the same. Return full contents of changed files only.
[paste files]
```

### C. Bug report
```
BUG
Where: [e.g. "second pit after the lanterns"]
What happened: [e.g. "I jumped early but still fell"]
What I expected: [e.g. "I should have cleared it"]
Phone/browser: [e.g. Android Chrome, fullscreen]
Screenshot: [attach if it's about how it looks]
Find the cause in the files below first, explain it in one sentence, then fix only that.
[paste files]
```

### D. Art swap
```
ART SWAP: replace [character/background] with the attached image.
The art pack is loaded by js/art.js using assets/art/frames.json ([x,y,w,h] per frame).
Keep hitboxes unchanged. Keep the procedural fallback if the image fails to load.
Keep pixel art crisp (nearest-neighbour, no smoothing). Background must stay light.
Tell me the exact filename and folder to upload the image to, and return the full
updated frames.json entry and any changed file.
[paste js/art.js and assets/art/frames.json]
```

### E. Verification checklist (Muse must answer every line before saying "done")
```
1. Files changed: [list]. Files NOT touched: [confirm].
2. Did I keep every locked rule? (sword, beam at full health, spin double jump,
   Deva music switch, mythology enemies, light backgrounds, no scanlines) yes/no each.
3. Did any existing feature get removed or renamed? (must be no)
4. Did I use any function/variable that is not in the files given? (must be no)
5. New numbers added to tuning.js with comments? 
6. Script load order in index.html unchanged?
7. What did I actually test vs only read through? Be honest.
8. What should Daniel check on the live site, step by step?
```

---

## 6. Daniel's step-by-step workflow with Muse (no code)

1. **Open a fresh Muse chat.** Paste prompt A.
2. **Paste the files it needs.** Open the file on GitHub (e.g. `https://github.com/thepodemail-lgtm/thirteenth-year/blob/main/js/player.js`), tap **Raw** or the copy button, paste into Muse. If you're not sure which file, use the table in section 2 or ask Muse first.
3. **Send one request** using template B, C or D. Read its one-line plan; reply 👍 or "change X".
4. **Paste the result into GitHub.** Edit link pattern:
   `https://github.com/thepodemail-lgtm/thirteenth-year/edit/main/<path>`
   e.g. `https://github.com/thepodemail-lgtm/thirteenth-year/edit/main/js/player.js`
   Sign in as `thepodemail-lgtm`, select all, delete, paste Muse's full file, tap **Commit changes**.
5. **Only change files Muse changed.** If it returns a file you didn't ask about, be suspicious.
6. **Check the live site.** Wait about 1 minute, open https://thepodemail-lgtm.github.io/thirteenth-year/ and hard-refresh (desktop: Ctrl+Shift+R; phone: close the tab and reopen, or add `?v=2` to the end of the link). Tap PLAY FULLSCREEN and play the changed part.
7. **If it broke:** on GitHub open the file → **History** → open the previous version and paste it back (or ask Game Dev to revert). Nothing is ever lost.

### The safe knob file: `js/tuning.js`
Edit link: https://github.com/thepodemail-lgtm/thirteenth-year/edit/main/js/tuning.js
Change only the digits; keep commas and brackets. Examples:
- `run: 104` (running speed), `jump: 318`, `djump: 280`
- `hearts: 5`, `lives: 3`, `heartsLostPerHit: 1`
- `bossHP: 40` (Vaka's health), `deva: 30` (Deva seconds)
- `musicVolume: 0.2`, `sfxVolume: 1.0`
- `dog.attackRange: 95`, `dog.barkGap: 3`
- `anim.bossRig: true` (false = Vaka as a still picture)

### Final safety net
Have Game Dev do the final check on anything a helper AI changed. Helpers sometimes say "done and tested" when it isn't.
