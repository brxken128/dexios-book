## Checksums

Hashing mode uses `BLAKE3` for verification, due to it's speed, security and regular updates. (very ideal for this use case).

We hash the **encrypted** file (after encryption and before/during decryption). This is to ensure that your file wasn't tampered with between encrypting it and decrypting it. If the hash isn't the same then something *very* bad has happened.

This was originally `sha3-512` in versions 3.x.x and below, and was `KangarooTwelve` in 4.x.x (via the `tiny_keccak` crate) but since v5 it has been changed to BLAKE3 for a number of reasons.

The `tiny_keccak` crate hasn't received updates in a long while, and is no longer actively maintained.

The `k12` crate is ideal for this situation - but it is rather immature compared to some other hashing implementations, so `BLAKE3` will be our main hashing algorithm, and there are no plans to change this as of yet.

`BLAKE3` also offered some *marginal* performance benefits, but this could be due to a number of factors.

## Standalone Hashing Mode

You can use this by running `dexios hash test.enc`. It can also be ran on any file you'd like - encrypted or not.

It can use memory or stream mode, and implements the same checking that `encrypt.rs` and `decrypt.rs` implement to automatically detect whether or not the file should be hashed in memory or stream mode.

There are optional `-s` and `-m` switches, but `-s` will redirect you to memory mode if your file isn't large enough. There is no way around this that doesn't involve changing the stream `BLOCK_SIZE` in `global.rs`. This will decrease performance of hashing, encrypting and decrypting.

The hashes provided by memory and stream modes are the same, and they are compatible with one another (only in standalone hashing mode).

### Performance

Tests were ran on a system with a Ryzen 7 3700x and 16gb of 3000MHz RAM - running Void Linux. The file used was originally 3.5GiB, and it was stored on a Cruicial MX500 SSD.

Version 6 removed JSON entirely, and dropped `base64`, which really shows in the performance metrics.

This is using AES-256-GCM.

The time was determined via `/usr/bin/time -f "%e"`

| Version     | -eHyk       | -dHyk       | Stream? |
| ----------- | ----------- | ----------- | ------- |
| 3.2.8       | 44.37s      | 40.91s      | No     |
| 4.0.0       | 23.70s      | 30.43s      | No     |
| 5.0.0       | 22.48s      | 28.66s      | No     |
| 5.0.2       | 20.14s      | 21.26s      | No     |
| 5.0.9       | 19.31s      | 18.92s      | No     |
| 6.0.0       | 11.74s      | 11.59s      | No     |
| 6.4.8       | 5.61s       | 5.35s       | Yes |