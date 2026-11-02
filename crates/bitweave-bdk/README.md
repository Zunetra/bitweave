# bitweave-bdk

The [bitweave](https://crates.io/crates/bitweave) wallet engine built on [`bdk_wallet`](https://crates.io/crates/bdk_wallet).

`BdkEngine` implements `bitweave-core`'s `WalletEngine` trait with BDK and SQLite. BDK is an implementation detail: no `bdk_*` type appears in the public API, and BDK only ever sees public descriptors. Signing keys stay with the caller.

**Most users want [`bitweave`](https://crates.io/crates/bitweave) instead**, which uses this engine by default.

See the [repository README](https://github.com/Zunetra/bitweave#readme) for the full picture. Licensed under [MIT](https://github.com/Zunetra/bitweave/blob/main/LICENSE).
