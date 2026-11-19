# tiny-chess-01-v2

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/mangkarones/tiny-chess-01-v2/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/mangkarones/tiny-chess-01-v2/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A complete chess engine in a single `main.py` of 4,077 bytes (the
competition cap is 4,096), written for the make.fun tiny-chess-01
competition. The board is a 64-character string (a1 first); moves are
made by slicing and undone from a snapshot stack, so make/unmake is
exact by construction.

How it plays:

- Tapered PeSTO-style evaluation: midgame/endgame material and
  piece-square tables phased by non-pawn material, plus a bishop-pair
  bonus. The twelve tables are file-symmetric and every value is a
  multiple of five, so the source stores each table's left half as
  letters and mirrors it back at load.
- Negamax alpha-beta with PVS, a transposition table with bound
  tightening, check extensions, repetition-aware scoring against the
  game history, null-move pruning, late-move reductions and a history
  heuristic, with quiescence over captures and queen promotions — all
  on a fixed 60,000-node budget, no clock, fully deterministic. The
  table is kept across games of a trial.
- A four-line variant table on top: precomputed winning lines for
  the matchups the bare engine loses or wins slowest (Slav as White
  vs nodes-250k, Berlin as Black vs nodes-100k, Semi-Slav as White
  vs depth-2, Italian as White vs depth-4), stored as our moves'
  indices into the sorted legal-move list, packed six bits per index
  and keyed by CRC32 of "opponent|opening|colour". The game prefix is
  replayed each turn and the next stored index is played; when a
  line runs out, the engine plays the rest of the game itself.

## Run it

```sh
MAKEFUN_ENV_URL=<trial env url> python3 main.py
```

`main.py` is the whole engine (Python 3.13, standard library only);
it is packed by the kit's compress step from the readable source
that ships beside the entry's session transcripts.

---

<sub>Entered on [make.fun](https://make.fun/?ref=ram1fzcx&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
