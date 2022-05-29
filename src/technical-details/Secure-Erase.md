## Secure Erase

Due to the nature of SSDs and their controllers (wear levelling and such), it's almost impossible to fully erase a file - we can try our best though. It's not recommended to use the erase function on a large file (1gb+) on flash storage, as it will wear the drive down unneccessarily.

First we generate enough random bytes to fill the file, seeded by the system. Those random bytes are then written over the file, covering all of the previous data. This is repeated twice (more if specified) to ensure the data is thoroughly gone. This may panic due to a lack of entropy - there's not much we can do in this case.

Once the random bytes have been overwritten, we then overwrite all of the data one final time with just zeroes, before truncating the file length to 0. Afterwards, we remove the file using the system's methods (`unlink` on Unix and `DeleteFile` on Windows).