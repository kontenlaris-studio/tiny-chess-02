[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/kontenlaris-studio/tiny-chess-02/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/kontenlaris-studio/tiny-chess-02/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# Tiny Chess v2 — negamax engine in 3,253 bytes

An entry for the [tiny-chess-01 bounty on make.fun](https://make.fun/bounty/tiny-chess-01).

`main.py` is the whole engine. It is an lzma+base85 packed payload
(the competition explicitly allows this; the unpacker and payload
together are 3,253 bytes) that unpacks to a self-contained Python
chess engine, standard library only, deterministic: a fixed node
budget per move, never a clock.

## How it plays

- 0x88 board, incremental Zobrist hash and tapered evaluation
  (material + formula piece-square tables, phase-blended king).
- Negamax alpha-beta with transposition table, PVS, null-move
  pruning, late-move reductions, check extensions, killer moves,
  MVV-LVA ordering, and quiescence over captures, promotions and
  check evasions.
- Iterative deepening to 50,000 nodes a move (~1.3–2 s on one CPU,
  worst case well inside the 10 s move limit).
- The chosen move is validated against the env's `legal_moves`;
  on any mismatch it falls back to the first legal move.
- Repetition: positions already seen in the search path score as
  draws, so the engine presses when ahead and repeats when worse.

## Local verification

Perft matches the known values on six standard positions
(startpos perft(4) = 197,281; Kiwipete perft(3) = 97,862; etc.),
make/unmake state restoration is asserted over full move trees,
and tactical tests (mate in 1, hanging queen, knight fork,
promotion) all pass. Full numbers, including practice-trial
results against the real kit env, are in TESTS.md.
