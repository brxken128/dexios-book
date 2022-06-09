## Auditing

### Official Audit

We currently have no plans to purchase an official audit from a company, as we just do not have the money. They're expensive, and they're only "valid" for a snapshot of your code, so it's probably not worth it for us.

We do use audited encryption libraries, so we're probably covered on that front. Rust doesn't allow for most issues thanks to it's compiler and lints, and with a bit of thought, we can prevent incorrect usage of these libraries.

### Auditing Yourself

If anyone would like to take a dive into the code, and ensure that cryptography-related functions are written correctly, you may find items of interest within these directories:

* `src/stream.rs` - this handles STREAM initialisation, encryption and decryption
* `src/cipher.rs` - this handles "memory" initialisation, encryption and decryption
* `src/key.rs` - this handles salt generation and `argon2id` hashing
* `src/primitives.rs` - these are the constants used, and it contains a function for generating the nonce
* `src/header.rs` - this contains functions for serialising/deserialising/getting the AAD of headers
* `src/protected.rs` - this contains our `Protected<>` wrapper that implements zeroize-on-drop, to erase secrets from memory once they're no longer in use

I'd highly recommend taking a peek into `key.rs`, `stream.rs`, and `cipher.rs` - these are the main cryptographic files.

If you happen to find anything problematic, please [open a Github issue](https://github.com/brxken128/dexios/issues) or email `brxken128@tutanota.com`. This may be done anonymously.