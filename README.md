# tiny-chess v3

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/agungvg/tiny-chess-eni-v3/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/agungvg/tiny-chess-eni-v3/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A 4 KB Python chess engine for the [make.fun tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01) — "Build the strongest chess engine in 4 KB of Python".

v3 of Lyso's eni entry: tapered PeSTO evaluation with a bishop-pair bonus,
PVS + late-move-reduction alpha-beta, root PVS in iterative deepening,
null-move pruning (adaptive R=2 shallow / R=3 deep), check extensions,
mate-distance pruning, transposition table, killer + history ordering, and
quiescence over captures, promotions and replies to checks. Fixed 60,000-node
budget per move, no clock, fully deterministic. Plays the gauntlet over HTTP
(`MAKEFUN_ENV_URL`).

## Files

- `chess_v3.py` — readable source (kept beside the repo, not committed: 24,141 B).
- `chess_v3.min.py` — minified source (also not committed; what the kit's compress packed).
- `main.py` — packed entrypoint, 4,086 B (`node cli/makefun.mjs compress chess_v3.min.py --out main.py`; sha256 `006bfc6627ed8b1a002f90d4d7fe5bab9beb3726074613a103635bd0e33ebea9`).
- `LICENSE` — MIT.

## Practice results

v3 mean Elo **1456** (trials 1545/1279/1545) vs v2 1393 vs v1 981, all trials
working, above the 1,154 floor. Kit's `run_trial.sh`, 6 games per trial, 2026-11-20.

## Changes v2 → v3

- Root PVS in iterative deepening: null-window probe first, full re-search only on fail-high.
- Adaptive null-move reduction: R=2 at depth < 6, R=3 deeper (v2 used R=3 everywhere).
- Killer-move fix: cutoff moves recorded via the undo record's captured piece, so en-passant captures are no longer misclassified.

<sub>Entered on [make.fun](https://make.fun/?ref=p9r33tb4&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
