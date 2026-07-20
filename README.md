# 😈 SEZZLE DEVIL

> A cheeky, phone-first **troll-platformer** where the *Devil* is the chaos of paying for things the **old way**, and every Sezzle product is a **power-up**. Collect your installments, dodge fake fees, and beat the devil.

**One file. No build. No dependencies. Runs offline.** Play it live, or just open [`sezzle-devil.html`](sezzle-devil.html) in any modern browser.

**▶ Play now: https://sanchesfer.github.io/sezzle-devil/**

---

## Table of contents
- [What it is](#what-it-is)
- [Play it](#play-it)
- [Controls](#controls)
- [The concept: Sezzle mechanics *are* the gameplay](#the-concept-sezzle-mechanics-are-the-gameplay)
- [Difficulty regions](#difficulty-regions)
- [The 10 levels](#the-10-levels)
- [Signature design idea: "knowledge is the power-up"](#signature-design-idea-knowledge-is-the-power-up)
- [Architecture](#architecture)
- [Level authoring guide](#level-authoring-guide)
- [Adding a new trap or power-up](#adding-a-new-trap-or-power-up)
- [Contributing](#contributing)
- [Roadmap / ideas](#roadmap--ideas)
- [Credits & disclaimer](#credits--disclaimer)

---

## What it is

Sezzle Devil is a portrait, phone-first platformer built with **vanilla JavaScript + `<canvas>`**. Everything is drawn with shapes and hand-authored vector paths (including a traced Sezzle logo mark). **No images, no fonts, no libraries, no network calls.** The entire game (engine, art, audio, 10 levels) lives in a single `sezzle-devil.html`.

The comedy is the surprise: a level looks safe, then betrays you. A floor drops, spikes erupt, a "free" bridge is a lie, a credit check springs at the finish line. Each betrayal maps to a real "old way" money pain, and each Sezzle product is the tool that lets you beat it. Tone: playful, financially empowering, cute. Sezzle is always the hero, the imp is cute (not scary).

## Play it

- **Live (easiest):** open **https://sanchesfer.github.io/sezzle-devil/** on any phone or desktop.
- **Desktop file:** open `sezzle-devil.html` in a browser. Keyboard works (see controls).
- **Phone from the file (works with no internet):**
  - **AirDrop / send** the file to your phone and open it in Safari or Chrome.
  - **Or serve over your LAN:** from the folder run `python3 -m http.server 8000`, get your IP with `ipconfig getifaddr en0`, then browse to `http://<ip>:8000/sezzle-devil.html` on a phone on the same Wi-Fi.
  - **Fullscreen:** Safari, Share, *Add to Home Screen*, then launch from the icon.

> The repo also ships an `index.html` (an identical copy of `sezzle-devil.html`) which is the GitHub Pages entry point. A GitHub Actions workflow (`.github/workflows/pages.yml`) redeploys the live site on every push to `master`.

## Controls

| Action | Touch | Keyboard |
|---|---|---|
| Move left / right | big ◀ / ▶ buttons (bottom-left) | `←` / `→` or `A` / `D` |
| Jump | big **JUMP** button (bottom-right) | `Space`, `↑`, or `W` |
| **Power jump** (full height without holding) | **double-tap JUMP** | double-tap the jump key |
| Back to region select | **home button** (top-right of the HUD) | `Esc` |
| Menus / retry | tap the on-screen button | `Enter` |

The feel is intentionally tight: **variable jump height** (tap = short hop, hold = higher), **coyote time**, **jump buffering**, a snappy fall, and multi-touch (move and jump at once). A quick **double-tap on JUMP** reaches the same full height as holding, so a thumb tap never shortchanges you. On touch devices the canvas fills the whole screen and the translucent controls float over the underground band, below the play floor.

## The concept: Sezzle mechanics *are* the gameplay

| Sezzle thing | In-game power-up / mechanic | What it teaches |
|---|---|---|
| **Pay-in-4** (and **Pay-in-5** on a few levels) | installments shown as Sezzle logos; collect them all to open the exit portal | split a purchase into equal installments |
| **Payment schedule** | grabbing installments **in order** builds an on-time **streak**; a perfect run pays a bonus | on-time payments on a predictable cadence |
| **Virtual Card** | reveals **hidden/invisible platforms** so you cross an "impossible" gap | pay the Sezzle way almost anywhere |
| **Pay-in-Full** | a **dash** speed-boost to clear a section cleanly | settle early, cleanly |

And the **traps** are the old way (they look safe, then betray you):

| Trap | Old-way pain | Behavior |
|---|---|---|
| **LATE FEE** trapdoor | surprise late charge | solid-looking floor drops out when you step on it |
| **HIDDEN FEE** spikes | buried fees | flush ground erupts into spikes as you approach |
| **SURPRISE INTEREST** fake platform | 0% that isn't really 0% | looks solid, vanishes the instant you commit weight |
| **MINIMUM PAYMENT** shover wall | the debt cycle | pops out and flings you back toward the pit |
| **DEBT** lava/pit | falling into debt | instant death; fall in and retry |
| **CREDIT DENIED** (near-exit betrayal) | a surprise hard credit check at the worst moment | springs as you reach the *open* portal; on retry the payoff is Sezzle's **soft check** (no hard credit check, so it won't affect your credit score) |

## Difficulty regions

Pick a region on the title screen. **Same 10 levels**, escalating pain. Difficulty is a table of knobs in `DIFFS`:

| Region | Tier | Trapdoor delay | Fake delay | Spike rise | Spike range | Shove |
|---|---|---|---|---|---|---|
| **USA** | easy | 0.42s | 0.20s | slow | short | weak |
| **CANADA** | medium | 0.30s | 0.14s | normal | normal | normal |
| **QUÉBEC** | brutal | 0.16s | 0.08s | fast | long | strong |

**Québec** additionally:
- Trap labels switch to **French** (`FRAIS DE RETARD`, `FRAIS CACHÉS`, `DETTE`, and so on).
- Every level requires collecting a **CONFORMITÉ** compliance form before the exit opens.
- **Level 1 starts you falling over lava** (run right immediately or drop in).
- **Inverted controls** on levels **2, 6, and 9**: press right to go left and left to go right (a start toast and a `⇄` on the level name warn you). Québec is the only region where `hardMode === true`.

## The 10 levels

Designed as one-new-idea-per-level, tutorial to boss (levels 2, 4, and 8 are **Pay-in-5**):

1. **The Comfy Shortcut.** Jump plus installments; the "free crossing" floor is a LATE FEE trapdoor.
2. **Neon Night Market.** HIDDEN FEE spikes erupt; a scary-looking ledge is actually safe.
3. **Bridge Over Troubled Debt.** Virtual Card reveals a bridge; the mid-canyon "dare" stone is a fake.
4. **Fine Print Falls.** The grand SURPRISE INTEREST concourse vanishes; humble nubs are real.
5. **Payday Metronome.** Read the beat; moving platforms plus one "skipped beat" lie.
6. **Full-Send Boulevard.** PAY-IN-FULL dash across a corridor that drops if you linger.
7. **Minimum Payment Climb.** Shover walls; the "helpful boost" flings you the wrong way.
8. **The Big One.** A vanishing staircase over debt; read the tells.
9. **The Fine Print Gauntlet.** Every trap remixed; the early "exit" is a disguised trapdoor.
10. **The Final Statement.** Boss arena, all mechanics, dash the collapsing "PAID OFF" cascade.

## Signature design idea: "knowledge is the power-up"

Traps look like ordinary terrain the first time (a trapdoor is indistinguishable from real floor; a fake platform looks solid; spikes are hidden). The **first** hit is a pure surprise. Once a trap kills you it becomes `known` and renders its **warning tell** on every retry (a dashed outline, a `⚠ LATE FEE` tag, dotted spike markers). So: **die once, read the lesson, the trap becomes visible, beat it.** Death shows a one-liner plus the real Sezzle takeaway, then an instant checkpoint retry.

## Architecture

Single file, three sections inside one IIFE: `<style>`, DOM (`<canvas>` plus control buttons), and the `<script>` engine.

- **Adaptive resolution.** Gameplay is authored in a **450-wide** virtual space with `FLOOR = 700`. On touch devices the virtual height (`VH`) adapts to the device aspect ratio so the canvas fills the whole screen edge to edge, with the controls floating over the underground band below the floor. Desktop stays a fixed 450x800, letterboxed and centered.
- **Game loop.** `requestAnimationFrame`, `frame(t)` computes `dt`, then dispatches on a small **state machine**: `title | play | dying | clear | win`. `update(dt)` runs physics and systems only in `play`; each state has a `draw*` function.
- **Physics.** Direct-set horizontal velocity for snappiness; gravity split into `GRAV_UP`/`GRAV_DOWN`; variable jump via `CUT`; `COYOTE` and `BUFFER` timers; a double-tap "power jump" that reaches full held height. **AABB collision** resolves X then Y against a per-frame `solids()` list. Max jump **rise about 150px**, horizontal **reach about 160px**: the winnability budget every level respects.
- **Camera and parallax.** Horizontal camera follows the player and clamps to `worldW`; background is a layered gradient plus hills, drifting bokeh, and clouds, each at its own parallax factor. Screen shake offsets the world on hits and landings.
- **Systems.** Web Audio blips (`beep()` / `sfx`), particle bursts, dust, confetti, floating toasts, a combo/streak scorer, and a HUD (installment pips, streak bar, score, dash timer, home button).
- **Vector art and the logo.** A small icon library draws the world (`impHead`, `drawBolt`, `drawTrophy`, `rr()`, and friends). The Sezzle **"S" mark is traced from the official logo** and **baked once to an offscreen sprite**, then blitted with a single `drawImage` per draw (cheap on mobile, no per-frame gradients or `shadowBlur`). The logo is the installment collectible, the exit portal, the title hero, the HUD accent, and the death-card mark.
- **Levels are data.** `LEVEL_FACTORIES` is an array of **factory functions** (one per level) that return a fresh level object each time it loads, so per-run mutable state (`dropped`, `collected`, `known`) is never shared. `buildLevel(n)` picks the factory; `loadLevel(n)` wires it up and applies region tweaks.
- **Deploy.** GitHub Actions (`.github/workflows/pages.yml`) publishes the root of the repo to GitHub Pages on every push to `master`.

## Level authoring guide

Each level is a factory `() => ({ ... })`. Coordinates are in the 450-wide virtual space; ground sits at `y = FLOOR (700)`.

**Constructors** (all create plain objects the engine understands):

| Call | Makes | Notes |
|---|---|---|
| `P(x,y,w,h,kind)` | platform | `kind:'ground'` = flat, seamless floor (h about 120); otherwise a floating ledge (h about 22) |
| `TD(x,w,label,d)` | LATE FEE trapdoor | drops after `d` seconds of standing (defaults to the region delay); pass a fixed `d` for dash-corridors |
| `FK(x,y,w,label)` | SURPRISE INTEREST fake platform | looks solid, vanishes on contact |
| `HD(x,y,w,amp,speed)` | hidden platform (Virtual Card) | invisible and non-solid until the card is grabbed; `amp>0` makes it bob |
| `MV(x,y,w,h,axis,amp,speed)` | moving platform | `axis:'x'|'y'`; carries the player |
| `SHV(x,dir,triggerX)` | MINIMUM PAYMENT shover | fires when the grounded player passes `triggerX`; shoves `dir` (plus or minus 1); jump the trigger to avoid |
| `SP(x,w,label,baseTop)` | HIDDEN FEE spikes | rise from the floor as you approach |
| `LV(x,y,w,h)` | DEBT lava/pit | lethal on contact |
| `C(x,y,order)` | installment (Sezzle logo) | **4 or 5 per level**, `order` runs `0..N-1` |
| `CARD(x,y)` / `DASH(x,y)` / `PUCK(x,y)` | Virtual Card / Pay-in-Full / hidden hockey-puck bonus | |
| `CMP(x,y)` | Québec CONFORMITÉ form | required only in Québec |
| `CP(x)` | checkpoint | must sit on ground |
| `EX(x)` | exit portal | opens when all installments (and, in Québec, the form) are collected |

**Minimum level shape:**

```js
() => ({
  name:'Level N · Title', worldW:2000, spawn:{x:60,y:FLOOR-P_H},
  platforms:[ P(0,FLOOR,700,120,'ground'), P(900,FLOOR,1100,120,'ground') ],
  trapdoors:[], fakes:[], hidden:[], movers:[], shovers:[], spikes:[], lava:[],
  coins:[ C(200,648,0), C(500,648,1), C(1100,600,2), C(1500,648,3) ],
  card:null, dash:null, puck:PUCK(1300,560), compliance:CMP(1600,648),
  checkpoints:[ CP(650) ], exit:EX(1750),
  denial:true,        // optional: CREDIT DENIED betrayal at the portal
  invert:true,        // optional: inverted controls (Québec only)
  tips:DTIPS,
})
```

**Rules of thumb**
- Keep jumps inside the budget: **rise up to about 150px, gap up to about 160px.** Leave margin on required precision jumps.
- Use **4 or 5 installments**, `order` `0..N-1`, laid out roughly left to right so the streak reads naturally. Every count-specific bit of UI reads from `L.coins.length`.
- Checkpoints and the exit must be on **ground** platforms.
- Every level introduces (or remixes) **one** signature idea; the tell of a new trap should be distinct from its neighbors.
- Add `denial:true` for the near-exit "CREDIT DENIED" gag (retry spawns you right beside the portal for the payoff).
- Add `invert:true` for the Québec inverted-controls twist (only active when `hardMode`).
- To register the level, add the factory to `LEVEL_FACTORIES` and bump `LEVEL_COUNT` if you change the count.

## Adding a new trap or power-up

1. Add a **constructor** near the others (returns a plain object with a `kind`).
2. If it's solid, include it in `solids()`.
3. Add its **update logic** in `update(dt)` (trigger, animation, lethal check, then `die('yourtype')`).
4. Add a **renderer** in `drawWorld()` (and a "known" warning tell if it's a hidden betrayal).
5. Add a **lesson** to `DTIPS` keyed by the `die()` type so the death screen teaches the Sezzle takeaway.
6. If it should scale with region, read timing from `D()`.

## Contributing

Contributions welcome: new levels, new traps, art and juice, balance tuning, accessibility.

- **No dependencies, no build step.** Keep it a single self-contained HTML file that runs by double-clicking. No external assets, fonts, or network calls. Keep `sezzle-devil.html` and `index.html` identical.
- **Style:** match the surrounding code (compact, comment the *why*). Draw art as vectors. No emoji in the game world, and no em dashes in on-screen copy.
- **Test checklist before a PR:**
  - Open the file and check the browser console. It must be **error-free**.
  - Play the level you touched in **all three regions** (USA, Canada, Québec) and confirm it's **winnable** (no unreachable jump, no soft-lock, checkpoints reachable, exit opens).
  - Verify the new or changed trap has a fair **tell on retry**.
  - Sanity-check on a phone viewport (portrait). Controls shouldn't cover gameplay, and the approach to the exit should stay smooth.
- **PRs:** describe the change, which levels and regions you tested, and attach a short clip or screenshot if it's visual.

## Roadmap / ideas

- Persist best score and per-region completion (localStorage).
- A mute toggle and a "tap for fullscreen" prompt on mobile.
- More near-exit surprise variety (last-second spike, moving portal).
- Level editor and shareable level codes.
- More Pay-in-5 (or Pay-in-2) level variety.

## Credits & disclaimer

A playful, educational prototype celebrating the Sezzle way of paying: split into installments, 0% interest on Pay-in-4, your plan shown up front, and a soft credit check that won't affect your credit score. The imp is cute; Sezzle is the hero. Built as a single-file canvas game with vanilla JS.

This is a fun/demo project, not an official Sezzle product or financial advice. Fee, interest, and credit-check specifics vary by market and over time, so treat the in-game copy as flavor and confirm any real claims against Sezzle's official terms. Sezzle branding and names are used for an internal, illustrative game.
