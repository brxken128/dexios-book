# Welcome!

Welcome to the Dexios Book! Here you will find a lot of information about the project, including: how to use it, best practices, performance metrics and much more. Please view the list of pages to find what you're looking for!

## What is Dexios?

Dexios is a command-line file encryption utility, suitable for encrypting files before uploading them to a cloud service. It is written entirely in rust and contains no unsafe code (some dependencies may contain unsafe code, but they have received the correct audits and are deemed secure).

It uses `XChaCha20-Poly1305` encryption by default, or `AES-256-GCM` if you specify `--aes`.


By default, `BLAKE3-Balloon` is used for hashing your password. Alternatively, `argon2id` can be used to hash your key, by specifying `--argon` during encryption.

## Security Notices

The [AES-GCM crate](https://github.com/RustCrypto/AEADs/tree/master/aes-gcm) has received a security audit by NCC group, with no significant findings. You can view the audit [here](https://research.nccgroup.com/2020/02/26/public-report-rustcrypto-aes-gcm-and-chacha20poly1305-implementation-review/).

The [ChaCha20-Poly1305 crate](https://github.com/RustCrypto/AEADs/tree/master/chacha20poly1305) has received a security audit by NCC group, with no significant findings. You can view the audit [here](https://research.nccgroup.com/2020/02/26/public-report-rustcrypto-aes-gcm-and-chacha20poly1305-implementation-review/)

The `deoxys` crate does not have a formal audit, and it is still rather experimental. Due to how new Deoxys is, relatively speaking, it's extremely hard to find a good implementation. We opted to use one created by the same [RustCrypto Team](https://github.com/RustCrypto) that almost all of our other cryptographic dependencies rely on. You **will** experience decreased encryption and decryption performance while using Deoxys. **NOTE: As of v8.8.0, encryption using `Deoxys` has been temporarily disabled. We weren't happy with the performance and agreed it would be best to remove this functionality until it improves. You may still decrypt any files that were encrypted with `Deoxys`.**

We use `BLAKE3-Balloon` hashing for passwords (as of v8.7.0), and older versions used `argon2id` with good, hard parameters. Both hashing algorithms are regarded as secure, and both APIs are provided by the RustCrypto Team.

All other cryptographic functions are deemed secure - but they don't protect your data, so any vulnerabilities in them would not have a detrimental effect on the security and integrity of said data.

## The Defaults

The defaults used in Dexios are more than adequate for even the most paranoid of users.

By running the simple command `dexios -e input.txt output.enc`, you are using the following:

* `XChaCha20-Poly1305`
* `BLAKE3-Balloon` hashing
* Sensitive data being completely erased from memory
* A tamper-resistant header that is authenticated along with every block of encrypted data
* LE31 STREAM encryption

## Tested Operating Systems

| OS | Working? |
|----|----------|
Void Linux | Yes |
Fedora 35 | Yes |
Fedora 36 | Yes |
Ubuntu 20.04 | Yes |
FreeBSD 13 | Yes |
FreeBSD 14 | Yes |
Windows 10 | Yes |
Windows 11 | Yes |
Android 12 | Yes |
MacOS | Yes |

## Donating

If you like my work, and want to help support the project, feel free to donate! This is not necessary by any means, so please don't feel obliged to do so.

```
XMR: 84zSGS18aHtT3CZjZUnnWpCsz1wmA5f65G6BXisbrvAiH7PxZpP8GorbdjAQYRtfeiANZywwUPjZcHu8eXJeWdafJQFK46G
BTC: bc1q8x0r7khrfj40qd0zr5xv3t9nl92rz2387pu48u
ETH: 0x9630f95F11dFa8703b71DbF746E5c83A31A3F2DD
```

## Privacy

Dexios will **never** collect your data. No data is sent to any server, nor are any requests.

Everything is done locally on your hardware and your machine.

I encourage all able users to take a peek at [the source code](https://github.com/brxken128/dexios) and see for themselves.

You may find the cryptographic functions in [dexios-core](https://github.com/brxken128/dexios-core)

## Thank you!

I'd like to give a huge "thank you" to the entire [RustCrypto Team](https://github.com/RustCrypto) for their hard work within the Rust cryptography community. This program would not be possible without their countless hours of work and dedication.