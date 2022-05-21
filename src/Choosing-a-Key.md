## Choosing a Key

Use a strong password, if you're planning to use an entered one.

`dd` can provide a suitable keyfile if you'd prefer a higher level of security:

`dd if=/dev/urandom of=keyfile bs=1 count=4096` for a 4kb random keyfile

If you lose your file's key/keyfile, there is no recovering the encrypted data. This is not unique to Dexios - it's the nature of encryption as a whole.

## Key Inputs

The priority is as follows:

1. First, Dexios will check for whether or not you have specified a keyfile (via `-k` or `--keyfile`)
2. If no keyfile is detected, it will look for the `DEXIOS_KEY` environment variable
3. If neither of the above are found, you will be shown a prompt to enter a password manually

The priority above is thrown out of the window if `-p` is specified, as this will prompt you for a password (even if the environment variable is set).