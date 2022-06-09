## Password Hashing - Managed by `dexios-core`

Password hashing is done with `argon2id`. It uses a 16-byte salt to derive the encryption key. Once a key has been hashed, it is dropped and zeroed out via the `zeroize` crate to ensure it stays in memory no longer than required.

### Handling the Hash

The hash is wrapped in `Protected<>`, which means it can only be exposed when called for in the program. It is only ever exposed to the cryptographic ciphers, and it is dropped right after. We use a custom `Protected<>` wrapper which implements zeroize-on-drop, to ensure that all sensitive information is removed from memory once it is no longer required.
