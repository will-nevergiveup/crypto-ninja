# Crypto Ninja

A single offline HTML file (`index.html`, ~114 KB). It makes no external requests: fonts are system stacks, and the coin and bomb art is inlined SVG. Open the file directly, or host it on any static site such as GitHub Pages.

## How to play
- Swipe across the coins (BTC, ETH, USDT, BNB, XRP, USDC, SOL, TRX, ZEC, HYPE) to slice them. Each coin is worth +1.
- Slice 3 or more coins in one stroke for a combo, which is worth +N bonus. Combos escalate:
  - 3–4: COMBO
  - 5–7: MEGA
  - 8–11: WHALE
  - 12+: TO THE MOON
- Every coin that falls off unsliced costs a life. The 3 X marks at the top right turn red as you lose them. Bombs that fall off cost nothing.
- Slicing a **RUG** bomb ends the game immediately.
- To start, slice the hero coin or press the **SLICE TO START** button.
- The start screen also has the difficulty (Easy/Normal/Hard), sound (off by default) and language (11 languages) controls.
- The game speeds up the longer you play.
- Pause with ⏸ (or P / Esc). The pause menu has Resume and Main menu. The game also pauses automatically when the tab is hidden or the app is switched away.
- Your best score is saved per difficulty in localStorage under `cninja.best.<diff>`.

## URL parameters
These are also documented in the comment at the top of `index.html`.

```
lang=en|zh-Hant|zh-Hans|ko|ja|th|fr|es|it|de|ru
diff=easy|normal|hard
demo=1          AI plays (attract / QA)
combo=N         with demo: a cluster of N coins (3..12; 1 = 6) sliced in one stroke
bomb=1          with demo: the AI slices a bomb at ~3.4 s
screen=start|play|over   reason=bomb|miss   score=N   lives=1..3
seed=N  freeze=MS  quality=auto|high|med|low  rm=1 (reduced motion)
safe=T,R,B,L    fake safe-area insets   fps=1   god=1 (misses free)
```

## Build and tools
- `src/*` holds the source parts. `tools/build.sh` concatenates them into `index.html` and then runs the coin inliner.
- `node tools/inline-coins.js [index.html] [design-dir]` reads `design/coins/<SYM>.svg` and `design/bomb.svg`, minifies them, and rewrites the `/*COIN_SVG_START*/…/*COIN_SVG_END*/` block.
  - To update the art, drop new SVGs in with the same names and re-run the inliner.
  - The `_L`/`_R` half files are ignored, because the halves are clipped on canvas using the STYLE.md §4 recipe.
- `NODE_PATH=<puppeteer-core>/node_modules node tools/shots.js [shots] [touch] [errors] [soak]` runs headless Chrome QA. It writes `shots/*.png` and `shots/qa.json`.

## Art status
- All 10 coins and the bomb use the Designer's SVGs from `design/`.
- HYPE is the Designer's own approximation of the logo.
- The canvas placeholder coin and bomb are only fallbacks, used if an SVG is missing or fails to decode.
- The background, effects and HUD are drawn in code following STYLE.md.
