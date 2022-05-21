## Stream Encryption

Files encrypted in streaming mode will need to subsequently be decrypted in streaming mode. **The streaming/non-streaming modes are not interchangable.**

Streaming mode can be enabled with `-s` (as of version 6.4.7, it's the default).

Streaming mode cannot be used on files with sizes <= 1mb (it will redirect your encryption/decryption to memory mode automatically).

## Memory Mode

As of version 6.4.7, the previous "normal" mode has been renamed to memory mode. Streaming encryption has been optimised and it is lighter on system resources, and a fair amount faster (it took roughly half the speed to encrypt a file, in my testing). 

It can still be used manually with `-m`, and support will not be removed, but it will no longer receive performance updates. There is rarely a need to use memory mode manually, as Dexios will detect if your file requires it (based on the size of the file).

Memory mode will be automatically used if your input file size is <= 1MB. This is because the stream block size is 1MB, and anything less would not make up a full block. There is no need to specify it manually in this case, as Dexios will redirect you to the correct mode and alert you of the change.