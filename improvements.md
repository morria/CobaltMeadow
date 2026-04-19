# Improvements

A playtest pass across every game. Top 10 impact items first, then
per-game notes and cross-cutting ideas.

## Top 10 (ordered by impact on fun + beauty)

1. **Unified sound-effects library.** Only some games have music, and
   none have consistent SFX on click, hit, success, or failure.
   Build a shared Web-Audio helper (`fx.ding`, `fx.thud`, `fx.sparkle`,
   `fx.crash`) and wire it into every interaction. The games feel
   visually alive but audibly flat.

2. **Progress persistence via localStorage.** Every game resets on
   refresh. At minimum Lost in the Forest, Stranded, and Card Masters
   series should auto-save between visits, with a clear "Reset
   Progress" button. High scores too.

3. **Card Masters: replace `prompt()` for Jack / King.** Right now
   picking the multiplier pops the browser dialog — ugly and breaks
   the arcade vibe. Swap it for an inline numeric-keypad modal in
   the same gold-on-parchment style as the deck.

4. **First-run tutorial overlays.** Each game has ~3-5 key
   interactions players won't discover on their own (Wizard Battle's
   sealed cards, Forbidden Forest's ritual, Weirdness's green-masked
   planets). Add a translucent "tap anywhere to dismiss" overlay that
   points at each UI piece once, then remember dismissal in
   localStorage.

5. **Weirdness — RETURN scene subtle tell.** The "pick the smallest
   truly-blue dot" puzzle is nearly impossible without brute-forcing.
   Add a *very* faint greenish atmospheric ring to the masked dots
   and narrate the tell in the intro line. Keeps the puzzle
   interesting without feeling unfair.

6. **Forbidden Forest "?" help button.** The rules (shield, pinned,
   seals, ritual, three forms, food aura, tent duration) are dense.
   A `?` icon in the HUD opening a rules modal would save new
   players a lot of trial and error.

7. **City Marbles — repaired-track glow.** After you tap a red gap
   to fix it, the resulting gold segment is indistinguishable from
   an original piece. Add a ~1.2 s gold pulse + small sparkle so the
   player gets credit and understands cause/effect.

8. **Monster Trucks — apex slow-mo and crash debris.** Every run
   feels the same speed. A brief 0.3s time-dilation zoom-in at max
   height and debris/flame particles on crash landings would make
   the arcade feel like an actual monster-truck show. Confetti when
   landing the +1100 platform.

9. **Stranded — ship cross-section as a live dashboard.** The SVG
   rooms are static. Tint each module's fill by its current
   stockpile (greenhouse greens up when Food > 7, recycler blue
   deepens with Water, scrubber cyan-glows when O2 is topped). The
   ship becomes a second HUD and the game looks alive between turns.

10. **Shareable previews.** Add proper `<meta property="og:*">` /
    Twitter-card tags to every game plus a 1200×630 preview PNG and
    a favicon. Today a shared link shows the raw URL. Cheap fix with
    big pride dividends.

---

## Per-game notes

### The Arcade (index)
- Tiles fade-in on page load (staggered 50 ms).
- "NEW" pill on recently added games (track by date in JS).
- Favicon + Apple touch icon for the home-screen flow we already set up.

### Card Masters
- Inline J/K keypad modal instead of `prompt()`.
- Sparkle / slight shake on Queen flip — the game-ender deserves drama.
- Track best round in localStorage.
- Ace-of-Hearts/Diamonds could pulse briefly to sell the suit-vs-rank rule.

### Wizard Battle
- Brew should animate a cauldron splash, not just update a potion chip.
- Cast-spell zap along a line from wizard to opponent.
- Sealed-card pulse: gentle red outline heartbeat so the "✕"
  registers even when greyed.
- Sound: bubble for brew, zap for cast, chimes for heal.

### Monster Trucks / Monster Trucks 2
- Camera slight zoom at apex of backflip.
- Speed-trail particles behind the truck during ramp phase.
- On crash: flame/smoke puff + scoreboard shake.
- On platform landing: gold confetti shower.
- Level 2 loop: optional slight camera roll around the loop tangent.

### Forbidden Forest
- `?` help panel (modal with compact rules text).
- Active-player glow on the hp-bar area, not just the pouch border.
- Enemy-intent hints: tiny arrows on wolves/bears showing their next
  intended tile, which is derivable from their AI. Turns guessing into
  planning.
- Make tent-countdown badge bigger on the grid.
- On ritual, a short slime-absorption animation before the form swap.

### City Defense
- Phase-change banners should fade over 0.6 s with a color shift,
  not pop.
- Flyer picker screen: idle wing-flap animation per bird.
- Bird-hit damage: red radial flash from the bird, screen shake.
- Swarm drones should drop particles when destroyed.
- Underground dig: faint ambient dust motes in the stratum.

### Time Loop
- Platformer: add ~120 ms coyote time on ledge jumps so the first
  run isn't frustrating.
- Orbs in space phase: leave a short fading trail on collect.
- Boss slime: pop burst with green droplets on every blob deflect;
  satisfying feedback.
- Home scene: kid's TV should show scrolling text so the weird
  flash reads as a story beat, not a glitch.

### Weirdness
- Green-masked planets need a tell — see top 10 #5.
- Ship battle: alien destruction should be an explosion sprite,
  not just `element.remove()`.
- Reveal scene: planet morph could include a brief scanline/glitch
  flicker over the dialogue for dread.
- Rocket flight on Earth-scene walk prompt: once the button unlocks,
  the rocket gently exhausts white smoke to draw the eye.

### Stranded
- Per-room tint driven by resource levels (see top 10 #9).
- Morale-low border pulse on the whole frame when Morale ≤ 3.
- Trend arrows on each resource bar (small ↑/↓ if last 3 days
  improved/declined) to teach the decay curve.
- Random-event log line could come with a matching inline icon.

### City Marbles
- Repaired-track glow (see top 10 #7).
- Marble needs a specular highlight — it's currently a flat disk
  in 3D.
- Add a confetti burst when the marble reaches the finish flag.
- Missed-gap fall: marble should leave a short trail before
  disappearing into fog.

### Lost in the Forest
- Tree click chip cadence could speed up with rapid clicks
  (combo meter feel).
- Signal-fire button: progress arc inside the button showing how
  many feeds are left to win (1 feed = +4, so 25 feeds).
- Attack banner: add 200 ms screen shake when it appears.
- Rogue map: reveal an arrow trail between visited tiles so the
  route is visible at a glance.
- Victory screen: plane flyover animation.

---

## Cross-cutting polish

- **Haptic vibration** on touch: `navigator.vibrate(12)` for taps,
  `vibrate([40, 20, 40])` for damage, `vibrate([100, 50, 200])` for
  wins. Feels massively better on phones.
- **Consistent keyboard map:** Space = primary action, `R` = reset,
  `M` = music toggle, arrow keys = movement/meter-steer. Write it
  once in a shared util.
- **Pause support** for all real-time games (City Defense, Orb Rush,
  Marble Run, City Marbles, Stranded auto-ticks, Lost in the Forest
  attacks).
- **Low-end phone pass** on the 3D games (Orb Rush, Marble Run, City
  Marbles): drop shadow quality on small viewports, cap pixel ratio
  harder, reduce particle counts.
- **Share button** on win overlays: builds a URL-safe result and
  copies to clipboard ("I saved the city in 3:42 — play ARCADE").
- **Palette audit** for color-blind players: Forbidden Forest's red
  shield-pinned and green shield-ready are close; Monster Trucks'
  hull bar is pink-to-pink; etc. A quick sim pass would catch them.
