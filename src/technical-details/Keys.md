## Obtaining the Key

All keys are wrapped in `Protected<>`. This ensures that data does not leak, is not copied and is zeroed on drop. It only stays in memory for as long as it needs to.

The key is checked, once, to ensure that it is not empty.

Once the length has been validated, keys are then safely transported to the `argon2id` function. See [Password Hashing](../dexios-core/Password-Hashing.md) for more information.

### Reading from the Terminal

While reading from the terminal, the passwords are stored as `String`s. We use `termion` to handle password entry, this way your input is hidden.

On encryption, where you need to enter the password twice, they are compared. The `String` used for validation is safely zeroed out, and the original is consumed into a `Vec<u8>`.

On decryption, the password is only entered once, and it is consumed into a `Vec<u8>`.

The `Vec<u8>` containing the key is wrapped into a `Protected<Vec>`, which takes ownership of the value.

### Reading from a Keyfile

The keyfile is opened, and it's contents are read into a `Vec<u8>`. The `Vec<u8>` is then wrapped into a `Protected<Vec>`, which takes ownership of the value.

### Reading from Environment Variables

The `DEXIOS_KEY` environment variable is checked (to see if it exists) - if so, the contents are read into a `String`. It is then consumed into a `Vec<u8>`, which finally gets wrapped into a `Protected<Vec>`. This may seem cumbersome, but there are no copies or clones of the data, meaning it is safe.