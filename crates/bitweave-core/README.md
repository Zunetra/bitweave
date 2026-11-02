# bitweave-core

Engine-agnostic types, traits and errors shared by the [bitweave](https://crates.io/crates/bitweave) crates.

- **Domain types:** balances, UTXOs, transaction requests, recipients and sync reports.
- **Seams:** the `WalletEngine` trait, plus the chain traits `BlockSource`, `Broadcaster` and `FeeEstimator`.
- **Errors:** one enum per operation, such as `BuildTxError::InsufficientFunds { needed, available }`.
- **Test kit:** `MockChain`, an in-memory chain for tests.

It performs no IO and does not depend on BDK, so any engine or chain backend can implement it.

**Most users want [`bitweave`](https://crates.io/crates/bitweave) instead**, which re-exports everything here behind one `Wallet` type. Depend on `bitweave-core` directly only to write your own engine or backend.

See the [repository README](https://github.com/Zunetra/bitweave#readme) for the full picture. Licensed under [MIT](https://github.com/Zunetra/bitweave/blob/main/LICENSE).
