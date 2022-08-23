## Encryption

Encryption is done by default with [`XChaCha20-Poly1305`](https://tools.ietf.org/html/rfc8439), but Dexios also has options for [`AES-256-GCM`](https://en.wikipedia.org/wiki/AES-GCM-SIV) and [`Deoxys-II-256`](https://sites.google.com/view/deoxyscipher). Encryption uses a cryptographically-secure randomly generated 32-byte value. This is then encrypted using the key you provided (once it has been hashed). Once the master key is encrypted, it is stored within the header. You may read more about this [below](#stream-mode).

### Stream Mode

The XChaCha20-Poly1305 nonce is 20 bytes long, and the AES-256-GCM nonce is 8 bytes long when stored. The stream encryptor dynamically changes the nonce, based on a 31-bit little endian counter and a 1-bit "last block" flag. Those last 32 bits (4 bytes) are appended to the end of the nonce - making the total nonce length 24/12 bytes respectively. This is done by the stream encryptor to ensure that identical data will not be encrypted with the same nonce.

On encryption (as of v8.7.0), a completely random 32-byte "master key" is generated. The user's key is then used to encrypt the master key, and that is included within the header (along with the random nonce). This was implemented as it will allow us to add features such as changing a key, or allowing multiple keys to decrypt the same file.

On encryption, the [header](Headers.md) is serialised and written to the start of the file. Then, Dexios reads the source file in 1MiB blocks (1048576 bytes), and encrypts them using an LE31 stream encryptor. Each "block" is read, encrypted, written. The header is always the same size, so we can avoid having to serialise/deserialise the data with something such as JSON. This had a lot of [performance benefits](../Checksums.md#performance).

For decryption, the [header](Headers.md) is is read and deserialised. Next, Dexios reads the remaining part of the encrypted file in 1MiB blocks + 16 bytes, to account for the AEAD tag. Each "block" is read, decrypted, and then written.

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

Memory mode should only be used in *very* specific cases. Streaming mode works on files less than the `BLOCK_SIZE` (1MiB) and should be favoured.

In memory mode, the nonce is static and 24/12 bytes (XChaCha20-Poly1305/AES-256-GCM) long, as the data is encrypted all in one pass.

On encryption, the data is read from the source file and placed into a `Vec<u8>` before being encrypted. The [header](Headers.md) is written, and then the encrypted data is written to the file - in that order.

On decryption, the 16 byte salt (used for [hashing](Password-Hashing.md)) and the 24/12 byte nonce are read - in that order. Dexios reads the remainder of the file into a `Vec<u8>`, before decrypting it and writing the plaintext bytes to the output file.