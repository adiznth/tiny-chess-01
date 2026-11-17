# tiny-chess-01 — 4 KB chess engine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/adiznth/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/adiznth/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A real chess engine in 4,087 bytes of Python 3.13 (stdlib only), built for
[make.fun's "tiny-chess-01" bounty](https://make.fun/bounty/tiny-chess-01)
($500 in USDC).
`main.py` is a one-line LZMA+base85 unpacker; the decompressed source
(~8 KB) is a complete engine. It reads, imports and execs no other file.

## Design

- **Board**: 0x88, pseudo-legal move generation, make/unmake with full
  state (castling rights, en-passant square, halfmove clock, Zobrist hash).
- **Search**: negamax alpha-beta with iterative deepening to a **fixed
  node budget of 180,000 nodes** (never a clock — fully deterministic),
  max depth 8. Transposition table, 2 killer moves per ply, MVV-LVA
  capture ordering, quiescence search (captures/promotions, all moves
  when in check). Repetition and fifty-move draws scored 0 inside search.
- **Eval**: material (P 100, N 320, B 330, R 500, Q 900) + Michniewski
  piece-square tables (mirrored, 32 entries per piece) + bishop pair +30
  + tapered king tables (midgame/endgame by non-pawn material).
- **Speed**: ~35–50k nodes/sec, worst-case move ~5 s (limit 10 s).

## Local practice results (kit gauntlet, 3 trials, 6 games each)

| trial | Elo | W–D–L | wall |
|-------|-----|-------|------|
| 0 | 1364 | 3–1–2 | 1087 s |
| 1 | 1279 | 3–0–3 | 1048 s |
| 2 | 1451 | 3–2–1 | 1194 s |

**Mean practice Elo 1365** (entry bar 1154). Beats the depth-2/3/4
house engines 8/9, competitive with nodes-100k/250k, loses to nodes-500k.
Perft verified (startpos d4 = 197,281; Kiwipete d3 = 97,862).

## Files

- `main.py` — the engine (4,087 bytes).
- `LICENSE` — MIT.
- `README.md` — this file.
