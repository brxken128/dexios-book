## Headers

### Structure

The full header is 64 bytes - it contains relevant information about the encrypted data.

V1 Headers (v8.0.0 - v8.2.0):

* The first two bytes of each header *should* contain `0xDE` - this signifies that it is indeed a Dexios file.
* The next two bytes signify the header version - as of v8.0.0, this is `0x01`
* The next two bytes contain the encryption algorithm, such as XChaCha20-Poly1305
* The next two bytes contain the encryption *mode*, which is either stream or memory
* The next 16 bytes contain the salt used for `argon2id`
* We then have 16 bytes of empty space, this has the potential to store more information in later versions
* We then have the `n` byte nonce - this is determined by the encryption algorithm and encryption mode
* We finally have enough zeroes to reach 64 bytes - this is extra empty space that may be used in later revisions

V2 Headers (v8.3.0):

* The first two bytes of each header *should* contain `0xDE` - this signifies that it is indeed a Dexios file.
* The next two bytes signify the header version - as of v8.3.0, this is `0x02`
* The next two bytes contain the encryption algorithm, such as XChaCha20-Poly1305
* The next two bytes contain the encryption *mode*, which is either stream or memory
* The next 16 bytes contain the salt used for `argon2id`
* We then have the `n` byte nonce - this is determined by the encryption algorithm and encryption mode
* We then pad the headers with zeroes until we reach a total of 48 bytes
* The last 16 bytes contain a truncated SHA3-512 HMAC signature

V3 Headers (v8.4.0-v8.6.0):

* The first two bytes of each header *should* contain `0xDE` - this signifies that it is indeed a Dexios file.
* The next two bytes signify the header version - as of v8.4.0, this is `0x03`
* The next two bytes contain the encryption algorithm, such as XChaCha20-Poly1305
* The next two bytes contain the encryption *mode*, which is either stream or memory
* The next 16 bytes contain the salt used for `argon2id`
* We then have 16 bytes of empty space, this has the potential to store more information in later versions
* We then have the `n` byte nonce - this is determined by the encryption algorithm and encryption mode
* We finally have enough zeroes to reach 64 bytes - this is extra empty space that may be used in later revisions

V4 Headers (v8.7.0+):

* The first two bytes of each header *should* contain `0xDE` - this signifies that it is indeed a Dexios file.
* The next two bytes signify the header version - as of v8.7.0, this is `0x04`
* The next two bytes contain the encryption algorithm, such as XChaCha20-Poly1305
* The next two bytes contain the encryption *mode*, which is either stream or memory
* The next 16 bytes contain the salt used for `balloon` hashing
* We then have the `n` byte nonce used for encrypting the data - this is determined by the encryption algorithm (in LE31 STREAM mode)
* Next we have enough padding to reach 48 bytes into the header
* We then have the encrypted master key, which is 48 bytes (32 for the data, 16 for the AEAD tag)
* We then have the `n` byte nonce used for encrypting the master key - this is determined by the encryption algorithm (in standard cipher/memory mode)
* We finally have enough padding to reach a total of 128 bytes in length

### Authenticating the Header with AAD (v8.4.0+)

Headers are calculated as a byte array (including the padding), and provided as AAD (additional authenticated data) to **every block** in stream mode, and the whole block in memory mode.

On decryption, the whole header is read (including the padding), and it is provided as AAD to **every block** in stream mode, and the whole block in memory mode. The data will not decrypt if the header does not match, even if an empty byte is altered. This fully protects your file from tampering, as even a single change will result in the user being made aware.

We have decided to use AAD as opposed to SHA3-512 HMAC due to few things:

* HMAC cluttered up the codebase quite significantly. This is not good for a tool such as Dexios, as it'd become harder and harder to maintain
* HMAC required two additional dependencies, `hmac` and `sha3`. This could potentially increase the attack surface (although unlikely)
* HMAC required additional computational power - this is something a lot of older systems do not have

On the other hand, AAD requires hardly any further computational power, or cluttering of the codebase - it's already implemented by the AEAD crates that we use.

For V4 Headers, the AAD is generated based on the first 48 bytes (for salt, nonce, version, algorithm, mode information, padding) and then the padding from the end. Authenticating the padding isn't necessary, but it could help prevent some forms of tampering.


### Changing a Key (v8.7.0+)

You can provide keyfiles with the `-k` and `-n` switches (for old and new keyfiles respectively). You may provide just an old keyfile, and `--auto` for autogenerating a new passphrase. The combinations are there to allow for you to easily change any key.

The file is first opened, and the header is deserialized. Both keys are then obtained, and are subsequently [hashed](Password-Hashing.md).

Once both keys have been hashed successfully, the master key stored within the header is decrypted (with the nonce provided in the header), using the old key. It is then re-encrypted with your new key, and a new nonce, before being written back to the file.

Currently, only one keyslot is supported per file, but there are plans to add more in the future. This means you could decrypt easily using a keyfile, and have a backup password/another keyfile in case your primary one is lost.

### Stripping

Headers are stripped by zeroing out the first n<sup>1</sup> bytes of the file - this is where the header is stored. Headers are first deserialized to ensure you are stripping a valid Dexios header.

<sup>1</sup> This is dependent on the header version


### Dumping

Headers are dumped by reading/deserializing the header from the input file, and writing them to the output file. This is a protective measure to ensure that you are dumping a valid Dexios header.

### Restoring

Headers are restored by deserializing the header provided in the input file, and writing the header to the output file.

### Signing (v8.3.0 - replaced by AAD)

Headers are signed using your hashed encryption key.

On encryption, the headers are signed when the ciphers/stream ciphers are initialised, this way we only have to hash your key once. The key is never cloned within memory, and it is zeroed out (using `zeroize` and our [protected wrapper](Protected-Wrapper.md)) once the header has been signed.

The signature is generated from the first 48 bytes of the header - this is where all of the important information is enclosed.

### Verifying (v8.3.0 - replaced by AAD)

On decryption, the headers are verified when the ciphers/stream ciphers are initialised. Once again, the key is zeroed out once the header has been verified.

If a single byte of the header changes, decryption will not continue as the signature will not match. This is to prevent a malicious actor from tampering with the header. 

**If the signature doesn't match, but you're positive your key is correct, it is best to assume the header was tampered with.**