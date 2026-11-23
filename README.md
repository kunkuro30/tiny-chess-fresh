[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/kunkuro30/tiny-chess-fresh/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/kunkuro30/tiny-chess-fresh/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# tiny-chess-fresh

A complete chess engine in 4,038 bytes of Python — the whole entry is
`main.py`, an lzma-packed payload that unpacks and execs itself.

## How it plays

- **Board**: a 120-character mailbox string, sunfish-style. The position
  is rotated after every move so the side to move is always "us":
  uppercase pieces at the bottom, pawns marching up the board.
- **Evaluation**: incremental material + piece-square tables, updated in
  `make()`; the king's table is tapered by game phase (0–24) at the leaf.
  The tables are file-symmetric, so only files a–d are stored.
- **Search**: negamax with PVS, a transposition table (bound flags +
  best move), quiescence search with check evasions, null-move pruning,
  late-move reductions and a check extension. Move ordering is
  TT-move first, then MVV-LVA captures and promotions.
- **Determinism**: iterative deepening is driven by a fixed node budget
  (200,000 nodes per move) — never by a clock — so a replayed trial
  reproduces every game move for move.

Verified before submission: perft from the start position
(8,902 at depth 3; 197,281 at depth 4) and the Kiwipete position
(97,862 at depth 3) match the published counts exactly, on the packed
file itself.
