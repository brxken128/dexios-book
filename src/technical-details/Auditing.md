## Auditing

### Official Audit

We currently have no plans to purchase an official audit from a company such as Cure53, as we just do not have the money. They're expensive, and they're only "valid" for a snapshot of your code, so it's probably not worth it for us.

We do use audited encryption libraries, so we're probably covered on that front. Rust doesn't allow for most issues thanks to it's compiler and lints, and with a bit of thought, we can prevent incorrect usage of these libraries.

### Auditing Yourself

If anyone would like to take a dive into the code, and ensure that cryptography-related functions are written correctly, you may find items of interest within these directories:

* `src/encrypt/crypto.rs` - this handles stream and memory encryption
* `src/decrypt/crypto.rs` - this handles stream and memory decryption
* `src/key.rs` - this handles reading the key from certain inputs, and the hashing of said keys
* `src/streams.rs` - this initialises our STREAMs, for both encrypt and decrypt modes
* `src/header.rs` - this contains functions for serialising/deserialising/getting the AAD of headers
* `src/secret.rs` - this contains our `Secret<>` wrapper that implements zeroize-on-drop, to erase secrets from memory once they're no longer in use
* `src/global/global.rs` - this just matches the enum type, and runs the appropriate AEAD functions

I'd highly recommend taking a peek into `key.rs`, `streams.rs`, and both `crypto.rs` files.

If you happen to find anything problematic, please [open a Github issue](https://github.com/brxken128/dexios/issues) or email `brxken128@tutanota.com`. This may be done anonymously.