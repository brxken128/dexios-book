## Installing

To install Dexios, there are two main options.

Firstly, you can install via `cargo` with `cargo install dexios`, or you may download a binary from the release page (make sure you mark it as executable, otherwise it won't run!).

We offer binaries for both Windows and FreeBSD, but our tests run on Ubuntu so Windows-specific issues may not be picked up. Please [open a Github issue](https://github.com/brxken128/dexios/issues) if you encounter any.


### Linux/FreeBSD

To use `cargo` for installing, ensure you have `gcc` installed on your system.

You may install via `cargo` with the command `cargo install dexios`

### Windows

Please use Windows Terminal or another Terminal program, as `cmd` does not have support for the command-line icons used by Dexios.

You may also use `cargo` for installing, just run the command:

`cargo install dexios`

### Android

Dexios can be installed on Android devices, via [Termux](https://termux.com/). All features have been confirmed to work, but if you find any, please [open a Github issue](https://github.com/brxken128/dexios/issues).

Dexios was tested on Android 12, with Termux v0.118.0, but it should work on a wide range of devices.

To install, ensure you have `cargo` installed. You may install Dexios with the command:

`AR=llvm-ar cargo install dexios`

## Building Notes

`gcc` is required for building on Linux and FreeBSD.

Manually setting `RUSTFLAGS` is no longer required, as the AEAD crates we use from the [RustCrypto Team](https://github.com/RustCrypto) automatically detect, and take advantage of hardware cryptography primitives.

## Downloading and running a pre-compiled binary

The [Github Releases](https://github.com/brxken128/dexios/releases) page contains pre-compiled binaries, generated with Github Actions. These are ideal if you don't want to go through the hassle of building, and they should run on any system (provided the architecture matches). They are the exact same binaries from Github Actions, so we can ensure they haven't been tampered with. 

We encourage users to check the hash provided in the GA Workflow, and compare with the file you have downloaded.