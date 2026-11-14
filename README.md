# tiny-chess-01 entry

`main.py` is a complete chess engine in 4,087 bytes: negamax alpha-beta
search with quiescence, a transposition table, null-move pruning and
late-move reductions, driven by a fixed 50,000-node budget — never a
clock — so every game replays move for move. It plays each game through
the env's HTTP protocol and was measured locally against the kit's
practice gauntlet (see "Local measurement" below).

## The size rule

- `main.py` is the whole engine: at most 4,096 bytes as committed,
  Python 3.13, standard library only. It reads, imports or execs no other
  file in the repository.
- Every byte counts: comments, whitespace, docstrings.
- Compression is allowed and counted. A `main.py` that unpacks and execs
  a compressed payload (`lzma`, `zlib`) is fine; the payload and the
  unpacker are both part of its 4,096 bytes.
- The rest of the tree (this README, LICENSE) is at most 8,192 bytes in
  all, and no file over 4,096. MIT, BSD, ISC and 0BSD licences fit.
- Search to a fixed depth or node count, never a clock: a final
  contender's trials are replayed and must play the same moves.

## Grade it locally

You need Docker and the `makefun` CLI. Download the competition's dev
kit, unpack it, and from inside it run:

    makefun grade-local --kit . --bot <your repository> --driver docker

It plays one practice trial against the house engines, replays it, and
prints the Elo the scorer gives it. Before you submit, `makefun check`
runs the site's own pre-submit checks, size caps included.

## Local measurement

This build machine has no Docker, so the official grade-local sandbox
cannot run here. The entry was instead measured with the kit's own env
server and the official Elo anchors and formula: three practice trials
scored 3.0, 4.0 and 2.0 points of 6 (local estimates of Elo 1279, 1451
and 1099), and an 8-rung gauntlet in the final's format scored 3.0 of 8
(Elo about 1253), above the 1154 eligibility floor. These are local
estimates, not official ratings. Move generation is verified by perft
(15/15 positions), and the packed `main.py` is verified move-identical
to its readable source.
