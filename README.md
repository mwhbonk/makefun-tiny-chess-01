# Tiny Chess — 4 KB Engine

A compact, original chess engine in a single Python file (`main.py`, 3,650 bytes),
written for the make.fun tiny-chess-01 competition ("strongest chess engine in
4 KB of Python").

## Engine

- 0x88 board, full legal move generation (castling, en passant, promotion)
- Negamax with alpha-beta pruning and iterative deepening
- Transposition table (Zobrist hashing, seeded deterministic)
- Quiescence search over captures and promotions
- Move ordering: transposition-table move, MVV-LVA captures, killer moves
- Evaluation: material + piece-square tables + bishop-pair bonus
- Fixed node budget (50,000 nodes/move), fully deterministic — no clock,
  no randomness at play time

`main.py` is a small unpacker: the engine source is LZMA-compressed and
base64-encoded inside it (explicitly allowed by the competition rules), and
is decompressed and executed at import time. The unpacked engine is
6,261 bytes of original, stdlib-only Python 3.13 code.

## Verification

- Perft-tested against the standard suite (start position through depth 4:
  20 / 400 / 8,902 / 197,281; Kiwipete, positions 3–5 all exact).
- Played a full game against a mock environment server and won by checkmate;
  every move was accepted as legal.

## License

MIT — see `LICENSE`.
