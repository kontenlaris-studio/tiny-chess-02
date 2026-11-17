# Tests — tiny-chess-02

`main.py` = 4,044 bytes (lzma+base85 payload of a 7,871-byte source;
compressed payloads are explicitly allowed). Fixed 50,000-node
budget, no clock — deterministic.

## Unit tests (shipped artifact)

- Perft, all exact: startpos 8902 / 197281, Kiwipete 97862,
  pos3 43238, pos4 9467, pos5 62379. Make/unmake + Zobrist OK.
- Puzzles at game budget: mate-in-1 ×3, hanging queen, knight
  fork, promotion UCI `d7c8q` — all found. Same move on repeats.
- Speed (this VM, Python 3.12): 23k–39k nodes/sec → 1.3–2.1 s/move
  worst seen, ~1.3 s typical (limit 10 s/move). Practice
  trial wall: 268 s of 1,800 s allowed.

## Match tests — real kit env trials

Full practice trials through the kit's own env server (the exact
house engines, real protocol, trial-0 openings). The trial is
deterministic; the shipped result reproduced game-for-game.

| config | W-D-L | practice Elo |
|---|---|---|
| **shipped, 50k nodes** | **5-0-1** | **1,654** |
| 80k nodes (rejected) | 2-2-2 | 1,279 |
| verbose dev build, 50k | 2-3-1 | 1,364 |

Per rung (shipped): beat depth-2, depth-3, depth-4, nodes-100k and
nodes-250k, all by checkmate; lost only to nodes-500k. Prize bar:
beat the sunfish baseline's practice mean (1,365); final floor
1,154. The 80k budget scored worse and tripled wall time — cut.

## Honest estimate

≈1,600 practice Elo: wins vs the first four rungs, a coin flip vs
nodes-250k/500k, losses vs nodes-1M/2M in the 8-game final. A
sweep of nodes-500k+ is not realistic at 4 KB.
