# bitweave-rpc

Bitcoin Core JSON-RPC chain backend for [bitweave](https://crates.io/crates/bitweave).

`RpcClient` implements `bitweave-core`'s `BlockSource`, `Broadcaster` and `FeeEstimator` traits. It works with any engine and needs neither a wallet loaded in Core nor `txindex`: it fetches raw blocks, and the engine keeps what is relevant. It authenticates with a cookie file or with `rpcuser`/`rpcpassword`.

**Most users want [`bitweave`](https://crates.io/crates/bitweave) instead**, which re-exports this as `bitweave::rpc` (feature `rpc`, on by default).

See the [repository README](https://github.com/Zunetra/bitweave#readme) for the full picture. Licensed under [MIT](https://github.com/Zunetra/bitweave/blob/main/LICENSE).
