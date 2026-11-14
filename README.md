# Tiny Chess — 4 KB Engine

A compact, original chess engine in a single Python file (`main.py`, 3,983 bytes),
written for the make.fun tiny-chess-01 competition ("strongest chess engine in
4 KB of Python"). Version 6, rebuilt as a root-agent session (the v1 entry was
rejected only on the transcript usage check, not on the code).

## Engine

- 0x88 board, full legal move generation (castling, en passant, promotion)
- Negamax with alpha-beta pruning and iterative deepening
- Transposition table (Zobrist hashing, seeded deterministic)
- Null-move pruning (depth >= 3, not in check, no en passant pending,
  static eval >= beta; reduced search at depth - 3)
- Delta pruning in quiescence (a capture is skipped when stand-pat +
  victim value + 200 < alpha; promotions and en passant never skipped)
- Quiescence search over captures and promotions, and over all replies when in check
- Move ordering: transposition-table move, MVV-LVA captures, killer moves,
  root moves ordered by the previous iteration's scores
- Every root move searched on a full window (a null-window root was tried in
  v2–v4 and made move choice arbitrary — fixed in v5)
- Correct mate vs stalemate scoring; search-path repetition detection
  (repetition scores 0). Game-history repetition was dropped in v6 to fund
  null-move pruning, so the engine may repeat a game position and draw.
- Evaluation: material + piece-square tables + bishop-pair bonus
- Fixed node budget (50,000 nodes/move), fully deterministic — no clock,
  no randomness at play time

`main.py` is a small unpacker: the engine source is LZMA-compressed and
base64-encoded inside it (explicitly allowed by the competition rules), and
is decompressed and executed at import time. The unpacked engine is
6,855 bytes of original, stdlib-only Python 3.13 code.

## Verification

- Perft-tested against the standard suite (start position through depth 3:
  20 / 400 / 8,902; Kiwipete 48 / 2,039 / 97,862; position 3: 14 / 191 / 2,812).
- Mate-in-1 found in the standard test position.
- Practice trial vs the house gauntlet (kit trial-0, local env server):
  v6 scored 2 wins, 2 draws, 12 losses, Elo 989 (wins vs depth-3 and
  nodes-250k as White; draws vs depth-2 and depth-4 as White by threefold
  repetition); v5 scored 1 win, 15 losses, Elo 705. Mean think time
  2,153 ms, max 5,813 ms, no move over the 10 s limit.

## License

MIT — see `LICENSE`.
