# SigmaSpire — search + neural networks for Slay the Spire 2

SigmaSpire combines a small policy/value network with CombatSolver search. Think of the broad search-plus-neural-network idea familiar from AlphaGo, applied to a card roguelike; this is not an LLM choosing cards and it is not a claim to reproduce AlphaGo's algorithm.

## Current model snapshot

Release: **r60-c9-r51-20261105**. This is the latest stack actually deployed at publication time: **C9 combat policy/value + R51 joint long-term value**, integrated with the **R60** compatibility fixes. The larger R61 outside-value model is still under development and is not presented as a finished release.

- In combat, CombatSolver evaluates candidate action sequences; the network contributes policy ordering and bounded value corrections.
- Outside combat, forecasts and a learned joint value model inform routes, card/resource choices, shop planning and future capability.
- Model parameters remain fixed within each run. Search/native experience is used in offline training between versions.

The R59 development recording demonstrates an IRONCLAD solo A10 native first victory. **It is not a 100-game acceptance result or an established win-rate claim.** The R60 snapshot uses the same C9/R51 weights with later compatibility fixes.

## Download and inspect

Download and extract `sigmaspire-models-r60-c9-r51-20261105.zip`. It contains the deployed JSON weights, the C9 PyTorch training checkpoint, companion models, deployment bindings, reference model/feature code, and an SHA-256 manifest. Run:

```sh
python verify_snapshot.py
```

The verification uses Python's standard library. Reading JSON weights does not require PyTorch; the training code/checkpoint does. The R51 `src/r51_joint_value/inference.py` implementation performs numeric inference without training packages.

## Integration requirements

This is a **model/contract snapshot**, not a standalone game-playing application or a drop-in replacement for an unmodified upstream mod. C9 expects the matching modified CombatSolver host and feature codec. Keep the feature schemas and game/solver assembly identities in `manifest.json` and the deployment JSONs; do not simply remove identity checks when using another game version. R51's model is conditional on the observed policy/data, not a causal guarantee of a choice's win probability.

No game binaries, Steam files, saves, credentials, raw play logs or unreleased R61 experiments are included. JSON weights are copied byte-for-byte from the sealed deployment. Small network capacity and limited data/mechanism coverage remain current limitations.

## Attribution and rights

Combat search is based on [Torch1230/CombatSolver](https://github.com/Torch1230/CombatSolver). Its MIT license and third-party notices are preserved under `licenses/`. Other upstream projects and their existing license terms are not relicensed by this snapshot. Publication of these project artifacts does not grant additional rights to the game or upstream assets.

## Recording

- [First-victory edited recording](https://www.bilibili.com/video/BV1S7446fEEJ/)
- Full original-speed, uncut recording: 29m19s, 3440x1440 at 60 fps; video is distributed separately on Bilibili.
