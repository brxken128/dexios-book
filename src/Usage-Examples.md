To just encrypt a file:

`dexios -e test.txt test.enc`

To just decrypt a file:

`dexios -d test.enc test.txt`

To just erase a file:

`dexios erase test.txt`

To encrypt a file using `AES-256-GCM`:

`dexios -eg test.txt test.enc`

To encrypt a file, and show the hash of the encrypted (output) file for verification later on:

`dexios -eH test.txt test.enc`

To decrypt a file, and show the hash of the encrypted file beforehand (to compare with the hash generated above):

`dexios -dH test.enc test.txt`

To encrypt a file, and erase the original file:

`dexios -e --erase test.txt test.enc`

To use a keyfile for encryption:

`dexios -ek keyfile test.txt test.enc`

To encrypt all `.mp4` files in a directory, we can use `find`. This works a LOT better with a keyfile/environment variable key as you will have to input the password manually each time otherwise. It will append `.enc` to the end of your files. You can remove `-maxdepth 1` to make this run recursively.

`find *.mp4 -type f -maxdepth 1 -exec dexios -eyk keyfile {} {}.enc \;`

To encrypt all `.mp4` files in a directory, and remove the original files once encrypted:

`find *.mp4 -type f -maxdepth 1 -exec dexios -ey --erase -k keyfile {} {}.enc \;`

To decrypt all `.mp4.enc` files in a directory, and remove the `.enc` suffix:

`find . -type f -iname "*.mp4.enc" -exec sh -c 'dexios -dk keyfile "$0" "${0%.enc}"' {} \;`

Here is a screenshot of Dexios in action! The performance is great, and the checksums match meaning the file is exactly the same as it was before encryption.

![Dexios in action](assets/dexios-in-action.png)