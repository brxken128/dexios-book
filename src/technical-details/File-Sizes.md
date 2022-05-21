## File Sizes

In versions 5.x.x and below, the 3.5GiB test file was encrypted at 4.72GiB - this involved a lot of overhead for `base64` and a tiny amount with the JSON.

As of version 6, JSON and `base64` has been dropped entirely. This has reduced the file size down to be *marginally* higher than our 3.5GiB test file (284 bytes higher, to be exact).