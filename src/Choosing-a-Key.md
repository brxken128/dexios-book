## Choosing a Key

Use a strong password, if you're planning to use an entered one.

`dd` can provide a suitable keyfile if you'd prefer a higher level of security:

`dd if=/dev/urandom of=keyfile bs=1 count=4096` for a 4kb random keyfile

If you lose your file's key/keyfile, there is no recovering the encrypted data. This is not unique to Dexios - it's the nature of encryption as a whole.

## Options for providing a key

You may provide keys in a few ways:

* `-k` for giving path to a keyfile
* `--auto` to tell Dexios to [auto-generate a key](technical-details/Keys.md#autogenerating-a-key-v870)
* The `DEXIOS_KEY` environment variable
* A user-provided password (this can be chosen by just running Dexios, and providing none of the above)