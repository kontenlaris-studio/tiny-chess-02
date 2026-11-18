# Tests — tiny-chess-02 (v3)

`main.py` = 3,731 bytes: lzma payload of the 9,234-byte source
`engine.py`, packed with the official `makefun compress`.
Fixed 50,000-node budget, no clock — deterministic.

## Changes from v2

Kept, each verified on the kit practice trials: bishop pair
(+28 MG / +44 EG, counted incrementally); pawn file-centrality
and advanced-knight terms; history heuristic ordering;
null-move gated on eval >= beta; deeper LMR (r=2 for very
late quiet moves); mate-distance pruning; a swap (SEE)
evaluator — quiescence prunes losing captures, ordering
demotes them.

Rejected by measurement: futility and late-move pruning —
with them the same build scored 1,545 / 1,364 / 1,364.
## Practice trials (kit env, 6 games each)

| engine | trial-0 | trial-1 | trial-2 | mean |
|---|---|---|---|---|
| v2 shipped | 1,654 (5-0-1) | 1,364 (3-1-2) | 1,279 (2-2-2) | 1,432 |
| v3 | **1,809 (5-1-0)** | **1,545 (4-1-1)** | **1,654 (4-2-0)** | **1,669** |

v3 lost one game in 18, never lost to nodes-500k. Packed
main.py reproduced trial-0 game-for-game (wall 496 s).
Perft exact: startpos 8902 / 197281, Kiwipete 97862, pos3
2812 / 43238. Slowest move: 3.5 s.
