# Welcome!

Welcome to the Dexios Book! Here you will find a lot of information about the project, including: how to use it, best practices, performance metrics and much more. Please view the list of pages to find what you're looking for!

## What is Dexios?

Dexios is a command-line file encryption utility, suitable for encrypting files before uploading them to a cloud service. It is written entirely in rust and contains no unsafe code (some dependencies may contain unsafe code, but they have received the correct audits and are deemed secure).

It uses `XChaCha20-Poly1305` encryption by default, or `AES-256-GCM` with the `-g` switch. `argon2id` is used to generate the encryption key.

## Security Notices

The [AES-GCM crate](https://github.com/RustCrypto/AEADs/tree/master/aes-gcm) has received a security audit by NCC group, with no significant findings. You can view the audit [here](https://research.nccgroup.com/2020/02/26/public-report-rustcrypto-aes-gcm-and-chacha20poly1305-implementation-review/).

The [ChaCha20-Poly1305 crate](https://github.com/RustCrypto/AEADs/tree/master/chacha20poly1305) has received a security audit by NCC group, with no significant findings. You can view the audit [here](https://research.nccgroup.com/2020/02/26/public-report-rustcrypto-aes-gcm-and-chacha20poly1305-implementation-review/)

`argon2` is regarded as the most secure password hashing algorithm at the time being, due to how resistant it is to cracking - that's why we use it! It's especially ideal while dealing with low-entropy inputs, such as human-selected passwords.

All other cryptographic functions are deemed secure - but they don't protect your data, so any vulnerabilities in them would not have a detrimental effect on the security and integrity of said data.

## Note about this Book

This book is still under heavy development, so apologies if some sections seem a little lacking.

## Tested Operating Systems

| OS | Working |
|----|----------|
Void Linux | Yes |
Fedora 35 | Yes |
Fedora 36 | Yes |
Ubuntu 20.04 | Yes |
FreeBSD 13 | Yes |
FreeBSD 14 | Yes |