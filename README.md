# makefun-tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/harrierme/makefun-tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/harrierme/makefun-tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in under 4 KB of Python — 0x88 board, negamax with alpha-beta, PVS, LMR and a transposition table, practice Elo 1364.

## Run it

```sh
makefun grade-local --kit . --bot . --driver docker
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=szgskt7z&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>

# tiny-chess-01

A chess engine in under 4 KB of Python. `main.py` is an LZMA payload
(a latin-1 string literal, `# coding: l1`) holding the real engine;
it unpacks and runs it. No model calls, no clock, no other files:
search is to a fixed node budget, so every trial replays move for
move.

Engine: 0x88 board, negamax with alpha-beta, PVS, LMR, null-move
pruning, check extensions and a transposition table keyed by an
incremental Zobrist hash; quiescence over captures, promotions and
check evasions with delta pruning; iterative deepening to depth 7
under a 180,000 node budget per move. Evaluation is material plus
piece-square tables (char-encoded, left-right symmetric), bishop
pair and passed-pawn bonuses (incremental, via per-file pawn
bitmasks), updated incrementally on make/unmake, with an endgame
king table once the queens are gone. Repetition and the fifty-move
rule score 0 inside the search, from the game's own move history.
The driver talks to the env at `MAKEFUN_ENV_URL` (`/game`, `/move`)
and cross-checks its moves against the env's legal list.

## Grade it locally

From the kit directory, with Docker and the `makefun` CLI:

    makefun grade-local --kit . --bot <this repository> --driver docker

Local practice trials (kit env server + official scorer, no Docker):
trial-0 Elo 1364 (3W 1D 2L; wins vs depth-2, depth-4, nodes-250k),
trial-1 Elo 1279 (3W 0D 3L). Both above the 1,154 practice floor;
the house baseline averages ~1,365 on 8-game practice trials.

## Size

- `main.py`: 4,082 bytes (limit 4,096)
- Whole tree: under 8,192 bytes, no file over 4,096. MIT licensed.
