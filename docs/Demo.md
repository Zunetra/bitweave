# Demo / Presentation Notes

Speaker notes for presenting this library in about three minutes: what to say it does, and how to structure the delivery.

## Features

**Core wallet capability**
- Non-custodial: private keys never leave the caller; the database physically cannot hold one (BDK only ever sees public descriptors)
- Four address types: Legacy (BIP44), Nested SegWit (BIP49), Native SegWit (BIP84, default), Taproot (BIP86)
- Two ways to source keys: BIP39 mnemonic (12/15/18/21/24 words, with optional passphrase) or explicit descriptors - public descriptors give a watch-only wallet for free
- Balance split by spendability: confirmed / unconfirmed / immature (coinbase maturity aware)
- Full UTXO listing and transaction history
- Three coin-selection strategies: branch-and-bound (changeless-first), largest-first, oldest-first - switchable per-wallet or per-transaction
- PSBT building and signing, generic across every script type (one signing path, not one per address kind)
- Fee bumping (BIP125 RBF) - replace a stuck payment with a higher-fee version
- Offline support: sign and track a payment without a node, broadcast later when reconnected

**Chain layer**
- Pluggable backend via three small traits (`BlockSource`, `Broadcaster`, `FeeEstimator`) - ships a Bitcoin Core RPC client, swappable for anything (Esplora, a custom source)
- Block-based sync with real reorg handling - walks checkpoints back to find the agreement point, reapplies from there
- Works against a `MockChain` with zero network dependency, for tests or demos

**Engineering / robustness**
- One typed error enum per operation, structured data (not strings) - `InsufficientFunds { needed, available }`, not a message to parse
- Swappable engine: the whole wallet logic sits behind one trait (`WalletEngine`) designed for this library, so BDK is a replaceable implementation, not a hard dependency
- Tested against official BIP44/49/84/86 test vectors, plus property tests, plus real end-to-end runs against actual `bitcoind` including real reorgs

**Built on top (proof it's a real library, not a toy)**
- A CLI and full terminal UI, built using nothing but the public API
- Encrypted key storage (AES-256-GCM, Argon2id-derived key, random salt/nonce every save)
- Multi-wallet support with a shared, leased local regtest node

## How to Present It

Three minutes is tight: one idea has to land, not five. Lead with the architecture decision, not a feature tour.

**Opening line (10s) - a claim, not a description:**

> "This is bitweave, a non-custodial Bitcoin toolkit where the database is physically incapable of storing a private key - not by convention, by construction."

Specific, provable, and it invites "wait, how?" which is exactly what a 3-minute talk needs.

**Timing**

| Time | Content |
|---|---|
| 0:00-0:15 | Hook line above, then: "It's a Rust library - key management, addresses, UTXOs, coin selection, PSBT signing, fee bumping, chain sync - plus a CLI and TUI built entirely on it to prove the API holds up." |
| 0:15-1:15 | **The one architectural idea**: the crate graph, dependencies pointing inward, `bitweave-core` has zero BDK dependency - point at `Cargo.toml` if a screen is available. Then the key-custody trick: BDK gets public descriptors only, the private keymap is kept separately, signing uses raw PSBT signing. This is the "wow" moment; spend half the time here. |
| 1:15-2:15 | **Fast feature rundown**, rapid-fire, no elaboration: four address types, mnemonic or descriptors, three coin-selection strategies, typed errors not strings, reorg-aware sync, offline queuing, pluggable chain backend. State them like a list, don't explain each. |
| 2:15-2:45 | **Proof it works**: official BIP test vectors, real regtest end-to-end tests with actual reorgs, and the TUI as a living demo that it's a usable library, not just types. |
| 2:45-3:00 | **Close with the roadmap in one breath**: "v1 wraps BDK behind that trait; v2 replaces it with a native engine, and because callers only see the trait, that's not a breaking change." Stop there. |

**Delivery notes**
- Don't narrate every crate name; nobody retains "bitweave-bdk vs bitweave-rpc vs bitweave-core" from a spoken sentence. Say "five crates, dependencies point one way" and move on; let a diagram carry the names if slides are used.
- If a live demo is possible, run `cargo bitweave tui --demo` for 15 seconds instead of describing the TUI - one real screen beats a sentence.
- Cut the testing section first if running long; it's supporting evidence, not the point.

**On "is `BdkEngine` a BDK trait?"**
- `WalletEngine` is the trait, and it belongs to this library - it lives in `bitweave-core`, which has zero dependency on BDK.
- `BdkEngine` is a struct, not a trait, written for this library. It implements `WalletEngine`, and internally holds a `bdk_wallet::Wallet` to do the derivation/coin-selection/PSBT work.
- The accurate line: "We defined our own trait for what any wallet backend must do. `BdkEngine` is our implementation of that trait, and it happens to use `bdk_wallet` under the hood. Swapping BDK out later (the v2 plan) means writing a new struct behind the same trait - nothing above it changes."
