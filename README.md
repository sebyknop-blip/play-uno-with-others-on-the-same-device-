# 🎴 Pass & Play UNO Night

An UNO-style card game for one phone and a table full of kids. The phone goes
round the table; your cards only appear once **you** hold the reveal button,
and everybody can always see how many cards everyone else is holding.

Built as a single `index.html` — no build step, no server, no dependencies.

## How it plays

1. **Set up** — pick 2–6 players. Tap a face to change the animal, type a name,
   and tap 🧒 to turn any seat into a robot 🤖. One kid against two robots, two
   kids and one robot, all people — whatever mix you like. At least one seat
   always stays human, since somebody has to be holding the phone.
   Each robot then gets its own **😴 Easy / 🤖 Fair / 🧠 Smart** strip, so a
   little one and a big one can share a table with a robot each at their own
   level. The strength shows as a badge on the scoreboard all game.
2. **Hand-off** — the screen says *"Pass the phone to 🦊 Mia"*. The hand is not
   just hidden, it isn't in the page at all until Mia presses and **holds** the
   button for half a second. A quick accidental tap while the phone is in mid-air
   won't expose anything.
3. **Your turn** — cards you're allowed to play glow and wobble; the rest are
   greyed out and can't be tapped, so there is no such thing as an illegal move.
   Tap a card, or take one from the deck.
4. **Robots** play on their own with a short "thinking" pause, and their move
   stays on screen long enough to follow. Anything that happened while the
   phone was away is then listed on your hand-off screen, so you never miss
   why you suddenly have four more cards.
5. **Everyone's card count** is pinned to the top of the screen at all times —
   on every screen, including the hand-off — as a number plus a little stack of
   card edges to count. A player down to one card flashes gold.

### Kid-friendly choices

Deliberately not the tournament rulebook:

- **No UNO penalty.** Reaching one card shouts "UNO!" automatically with a
  banner and a jingle. Nobody gets punished for forgetting to say it.
- **No Draw Four challenge** — too much lawyering for a five-year-old.
- **The first card is always a plain number**, so the opening turn has no
  surprise rules.
- **Cards are sorted** by colour then number every time a hand is shown, so
  cards don't jump around between turns.
- **6 and 9 are underlined** and corner numbers are never printed upside-down,
  because an upside-down 9 reads as a 6.
- Playing to two players makes **Reverse act as a Skip**, per the real rules.

### How strong are the robots?

Measured, not guessed. Each row is 2500 complete games of one opponent playing
**completely at random** against two robots of that strength. Three players, so
an even split is 33.3%; ± is a 95% confidence interval.

| Level | What it does | Random opponent wins |
| ----- | ------------ | -------------------- |
| 😴 Easy | Any legal card at random, random wild colours, and a quarter of the time it doesn't spot its move at all and just draws | **74.4% ± 1.7** |
| 🤖 Fair | Random, but usually saves its wilds for when it is stuck | **29.9% ± 1.8** |
| 🧠 Smart | Attacks whoever is about to go out, plays from the colour it holds most of, keeps action cards in reserve, never wastes a wild, and heads-up always takes the action card that buys it another turn | **26.3% ± 1.7** |

A *random* opponent is a low bar, so a child who is actually thinking does
better than these numbers everywhere. Easy exists to be beaten; Fair is a real
game; Smart makes you work for it. Head to head over 600 games, Smart beats
Fair 58–42 and Easy 90–10.

Knobs at the top of the bot section: `EASY_DOZE` (how often Easy misses its
turn) and `BOT_FORGETFULNESS` (how often Fair forgets to save a wild).

## Playing it on an iPhone

Open the page in Safari, then **Share → Add to Home Screen**. It launches
full-screen with its own icon, no Safari chrome, and works offline once loaded.

The layout is portrait-first: it uses `dvh` units and `env(safe-area-inset-*)`
so the notch and home-bar don't clip anything, and it re-fits the hand whenever
Safari's toolbars slide in or out. Card sizes are chosen at render time so a
whole hand fits on screen without scrolling, however many cards you're holding.

## Running it locally

```sh
npm run serve     # then open http://localhost:8000
```

Or just double-click `index.html` — it works straight off the filesystem.

## Development

```sh
npm install
npm test          # drives the game in a headless iPhone-sized browser
npm run icon      # regenerates icon-180.png
```

`test.mjs` fuzzes complete games through the rules engine — 40 all-human plus 60
with robots, rotating through the strength levels and mixing two different ones
at the same table — checking that all 108 cards stay accounted for at every step and
that every game terminates. It then walks the real UI and asserts the things
that matter: that no hand is in the DOM during a hand-off, that no hand is on
screen at any frame while a robot is moving, that the scoreboard paints above
the hand-off overlay, and that nothing overflows the viewport.

## Files

| File                    | What it is                                  |
| ----------------------- | ------------------------------------------- |
| `index.html`            | The entire game: markup, styles and rules   |
| `manifest.webmanifest`  | Add-to-Home-Screen metadata                 |
| `icon-180.png`          | App icon (generated)                        |
| `make_icon.py`          | Draws the icon; pure stdlib, no Pillow      |
| `test.mjs`              | Rules fuzzing + UI checks via Playwright    |

---

Not affiliated with or endorsed by Mattel. UNO is their trademark; this is a
homemade family game that plays by similar rules.
