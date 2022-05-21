## Encryption

Encryption is done with `AES-256-GCM`, using the key derived from `argon2id`.

### Stream Mode

The XChaCha20-Poly1305 nonce is 20 bytes long, and the AES-256-GCM nonce is 8 bytes long when stored. The stream encryptor dynamically changes the nonce, based on a 31-bit little endian counter and a 1-bit "last block" flag. Those last 32 bits (4 bytes) are appended to the end of the nonce - making the total nonce length 24/12 bytes respectively. This is done by the stream encryptor to ensure that identical data will not be encrypted with the same nonce.

On encryption, the 16 byte salt (used for [hashing](#password-hashing)) and the 20/8 byte nonce are written to the start of the file - in that order. Then, Dexios reads the source file in 1MB blocks (1048576 bytes), and encrypts them using an LE31 stream encryptor. Each "block" is read, encrypted, written, and then hashed with BLAKE3 (if the option is selected). The salt and the nonce are always the same size, so we can avoid having to serialise/deserialise the data. This had a lot of [performance benefits](https://github.com/brxken128/dexios/wiki/Checksums#performance).

For decryption, the 16 byte salt (used for [hashing](#password-hashing)) and the 20/8 byte nonce are read - in that order. Next, Dexios reads the remaining part of the encrypted file in 1MB blocks + 16 bytes, to account for the AEAD tag. Each "block" is read, decrypted, written, and then hashed with BLAKE3 (if the option is selected).

Hashing was not implemented in stream mode in the same way it's implemented in memory mode (in regards to decryption) - this is because of performance issues (especially on slower disks). It would cause the encrypted file to be read twice in total, instead of just the once, so it was decided against. Reading a (possibly large) file from a HDD can be slow enough, and that's not including the second read. Caching and such could help, but it's not a guarantee and not something I'd personally be willing to rely on. Instead, the file is hashed at the same time as the decryption, and the hash is printed at the end. If the hash doesn't match - something really must've gone wrong.

### Memory Mode

In memory mode, the nonce is static and 24/12 bytes (XChaCha20-Poly1305/AES-256-GCM) long, as the data is encrypted all in one pass.

On encryption, the data is read from the source file and placed into a `Vec<u8>` before being encrypted. The 16 byte salt (used for [hashing](#password-hashing)), the 24/12 byte nonce and then the encrypted data are all written to the file - in that order. If hashing mode is selected, the salt, nonce and data will be hashed and the BLAKE3 hash will be printed.

On decryption, the 16 byte salt (used for [hashing](#password-hashing)) and the 24/12 byte nonce are read - in that order. Dexios reads the remainder of the file into a `Vec<u8>`, before decrypting it and writing the plaintext bytes to the output file. If hashing mode is selected, the salt, nonce and data will be hashed with BLAKE3 before it is decrypted and written - giving the user a prompt to continue with decryption (provided the hash matches).