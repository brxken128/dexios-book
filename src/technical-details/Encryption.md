## Encryption

Encryption is done by default with `XChaCha20-Poly1305`, but Dexios also has options for `AES-256-GCM` and `Deoxys-II-256`. Encryption uses the key derived from `argon2id`.

### Stream Mode

The XChaCha20-Poly1305 nonce is 20 bytes long, and the AES-256-GCM nonce is 8 bytes long when stored. The stream encryptor dynamically changes the nonce, based on a 31-bit little endian counter and a 1-bit "last block" flag. Those last 32 bits (4 bytes) are appended to the end of the nonce - making the total nonce length 24/12 bytes respectively. This is done by the stream encryptor to ensure that identical data will not be encrypted with the same nonce.

On encryption, the [header](https://github.com/brxken128/dexios/wiki/Headers) is serialised and written to the start of the file. Then, Dexios reads the source file in 1MB blocks (1048576 bytes), and encrypts them using an LE31 stream encryptor. Each "block" is read, encrypted, written, and then hashed with BLAKE3 (if the option is selected). The salt and the nonce are always the same size, so we can avoid having to serialise/deserialise the data. This had a lot of [performance benefits](https://github.com/brxken128/dexios/wiki/Checksums#performance).

For decryption, the [header](https://github.com/brxken128/dexios/wiki/Headers) is is read and deserialised. Next, Dexios reads the remaining part of the encrypted file in 1MB blocks + 16 bytes, to account for the AEAD tag. Each "block" is read, decrypted, written, and then hashed with BLAKE3 (if the option is selected).

### Stream mode - Edge Cases

Edge cases regardigng *very specific* block sizes have been checked and confirmed to be working.

The major concern was when a file contains bytes exactly divisible by `1048576`, but this is not an issue.

To test this, I created a file with exactly `3145728` bytes (3x streaming blocks). I then instructed Dexios to show me the `read_count` when encrypting, and it read the file 4 times - 3 times for each of the 3 blocks, and one containing 0 bytes. The 0 bytes were encrypted using the `encrypt_last` function, adding 16 bytes to the end of the file.

The total encrypted file size was `3145856` bytes - let's break it down.

* 64 bytes are for the Dexios header
* (16*3) are for the AEAD tag, 16 bytes per block

This brought our file size down to `3145744` bytes, which is a difference of 16 bytes compared to our source file. This confirms that the source data is encrypted completely, even in rather specific edge cases.

Using `dexios hash`, we can confirm that the file hashes are exactly the same before encryption, and after decryption.

### Memory Mode

In memory mode, the nonce is static and 24/12 bytes (XChaCha20-Poly1305/AES-256-GCM) long, as the data is encrypted all in one pass.

On encryption, the data is read from the source file and placed into a `Vec<u8>` before being encrypted. The [header](https://github.com/brxken128/dexios/wiki/Headers) is written, and then the encrypted data is written to the file - in that order. If hashing mode is selected, the header and encrypted data will be hashed and the BLAKE3 hash will be printed.

On decryption, the 16 byte salt (used for [hashing](#password-hashing)) and the 24/12 byte nonce are read - in that order. Dexios reads the remainder of the file into a `Vec<u8>`, before decrypting it and writing the plaintext bytes to the output file. If hashing mode is selected, the salt, nonce and data will be hashed with BLAKE3 before it is decrypted and written - giving the user a prompt to continue with decryption (provided the hash matches).