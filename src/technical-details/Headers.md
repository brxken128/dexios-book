## Headers

### Structure

The full header is 64 bytes - it contains relevant information about the encrypted data.

* The first two bytes of each header *should* contain `0xDE` - this signifies that it is indeed a Dexios file.
* The next two bytes signify the header version - as of v8.0.0, this is `0x01`
* The next two bytes contain the encryption algorithm, such as XChaCha20-Poly1305
* The next two bytes contain the encryption *mode*, which is either stream or memory
* The next 16 bytes contain the salt used for `argon2id`
* We then have 16 bytes of empty space, this has the potential to store more information in later versions
* We then have the `n` byte nonce length - this is determined by the encryption algorithm and encryption mode
* We finally have enough zeroes to reach 64 bytes - this is extra empty space that may be used in later revisions

### Stripping

Headers are stripped by zeroing out the first 64 bytes of the file - this is where the header is stored.

### Dumping

Headers are dumped by reading the first 64 bytes of the input file, and writing them to the output file.

### Restoring

Headers are restored by reading the first 64 bytes of the provided input file, and writing them to the first 64 bytes of the output file.
