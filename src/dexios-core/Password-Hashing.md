## Password Hashing

Password hashing was done with `argon2id` in all versions prior to v8.7.0. In versions v8.7.0 and above, BLAKE3-Balloon hashing is used. Both modes use a 16-byte salt to derive the hashed key. Once a key has been hashed, it is dropped and zeroed out via the `zeroize` crate to ensure it stays in memory no longer than required.

You are able to use `argon2id` for hashing as of v8.8.0, via the `--argon` switch.

### Handling the Hash

The hash is wrapped in `Protected<>`, which means it can only be exposed when called for in the program. It is only ever exposed to the cryptographic ciphers, and it is dropped right after. We use a custom `Protected<>` wrapper which implements zeroize-on-drop, to ensure that all sensitive information is removed from memory once it is no longer required.
