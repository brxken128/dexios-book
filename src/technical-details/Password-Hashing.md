## Password Hashing

Password hashing is done with `argon2id`. It uses a 16-byte salt to derive the encryption key. Once a key has been hashed, it is dropped and zeroed out via the `secrecy` crate to ensure it stays in memory no longer than required.

### Handling the Hash

The hash is wrapped in a `Secret<>`, which means it can only be exposed when called for in the program. It is only ever exposed to the `AesGcm` cipher, and it is dropped right after. The `secrecy` crate implements a zeroize-on-drop routine, where the data is safely zeroed once dropped.
