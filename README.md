# tiny-chess v3

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/agungvg/tiny-chess-eni-v3/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/agungvg/tiny-chess-eni-v3/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A 4 KB Python chess engine for the [make.fun tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01) — "Build the strongest chess engine in 4 KB of Python".

v3 of Lyso's eni entry: tapered PeSTO evaluation with a bishop-pair bonus,
PVS + late-move-reduction alpha-beta, root PVS in iterative deepening,
null-move pruning (adaptive reduction R=2 shallow / R=3 deep), check
extensions, mate-distance pruning, transposition table, killer + history
ordering, and quiescence over captures, promotions and replies to checks.
Fixed 60,000-node budget per move, no clock, fully deterministic. Plays the
gauntlet over HTTP (`MAKEFUN_ENV_URL`).

## Files

- `chess_v3.py` — the readable source (kept beside the repo, not committed:
  it is 24,141 bytes and would break the 8,192-byte snapshot cap).
- `chess_v3.min.py` — the minified source (also not committed; it is what
  the kit's compress command packed).
- `main.py` — the packed entrypoint, 4,086 bytes
  (`node cli/makefun.mjs compress chess_v3.min.py --out main.py`, run as its
  own command after the last source change; sha256
  `006bfc6627ed8b1a002f90d4d7fe5bab9beb3726074613a103635bd0e33ebea9`).
- `LICENSE` — MIT.

## Practice results (kit's own trial runner, trials 0–2, 6 games each)

| engine | trial 0 | trial 1 | trial 2 | mean Elo | W–D–L | spare plies |
|---|---|---|---|---|---|---|
| v1 (current live entry, 80k nodes) | 1191 | 876 | 876 | **981** | 1–7–10 | 141 |
| v2 (PVS+LMR, 60k nodes) | 1364 | 1451 | 1364 | **1393** | 10–2–6 | 1103 |
| **v3 (this entry, 60k nodes)** | **1545** | **1279** | **1545** | **1456** | **11–2–5** | **932** |

Measured 2026-11-20 ~06:09–07:10 UTC with the kit's `run_trial.sh`.
Every trial: `working: 1`, no timeouts, no crashes. v3 is above the
practice floor (1,154) on all three trials and above the house baseline's
practice mean (~1,365, sunfish 4K). Caveat: 6-game trials are noisy — the
v3−v2 paired differences per trial are +181 / −172 / +181 (mean +63), so
the v3 lead over v2 is measured but not decisive beyond noise; v3 is kept
because it is the best measured option and its changes are principled
(root PVS, adaptive null-move reduction, en-passant killer fix).

## Changes v2 → v3

- Root PVS: in iterative deepening, the first root move gets a full window;
  the rest get a null-window probe first and a full re-search only on
  fail-high. Same moves, fewer nodes per iteration.
- Adaptive null-move reduction: R=2 at depth < 6, R=3 deeper (v2 used R=3
  everywhere).
- Killer-move fix: a cutoff move is recorded as a killer when the captured
  piece is none (`cap` from the undo record); v2 tested the destination
  square after unmake, which misclassified en-passant captures as quiet.

<sub>Entered on [make.fun](https://make.fun/?ref=p9r33tb4&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
