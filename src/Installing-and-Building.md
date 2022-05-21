## Installing

To install Dexios, there are two main options.

Firstly, you can install via `cargo` with `cargo install dexios`, or you may download the binary from the release page (make sure you mark it as executable, otherwise it won't run!). 

To use `cargo` for installing, ensure you have `gcc` installed on your system.

For better performance, you may install via `cargo` with the command `RUSTFLAGS="-Ctarget-cpu=native -Ctarget-feature=+aes,+sse2,+sse4.1,+ssse3" cargo install dexios`.

## Building Notes

As mentioned in the [AES-GCM crate docs](https://docs.rs/aes-gcm/latest/aes_gcm/index.html#performance-notes), please enable certain flags while building. For example:

`RUSTFLAGS="-Ctarget-cpu=native -Ctarget-feature=+aes,+sse2,+sse4.1,+ssse3"`

Change native to whichever CPU family/model you are going to be running the code on, if it's going to be ran on a different machine.

`gcc` is required for building.

## Downloading and running a pre-compiled binary

The [Github Releases](https://github.com/brxken128/dexios/releases) page contains pre-compiled binaries, generated with Github Actions. These are ideal if you don't want to go through the hassle of building, and they should run on any system (provided the architecture matches). They are the exact same binaries from Github Actions, so we can ensure they haven't been tampered with. 

We encourage users to check the hash provided in the GA Workflow, and compare with the file you have downloaded.