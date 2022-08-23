## Directory "Packing"

After using the pack feature to create an archive, it is just a zip file. You may decrypt this as normal and open it for yourself, if you so wish.

Folders encrypted with this mode have all of their information hidden. There is no way for anyone to deduce how large each file was, or their names/contents, once it has been encrypted.

This mode does not preserve metadata, file attributes or permissions. It also does not follow symbolic links.

### Usage Examples

To pack an entire directory (recursively):

`dexios pack -r dir/ dir.enc`

To unpack the directory (and output it to your current path):

`dexios unpack dir.enc .`

### Creating an Archive

First, the files and directory structure are both indexed. While indexing, if any files/directories match an "excluded" pattern, they will be ignored.

The zip file is then initialised with a random 8 character alphanumeric extension, this is to prevent possible endless-loops (the buffer kept getting filled with the data that was just written).

The directory structure is recreated within the zip file (if in recursive mode, this will include other directories and not just the source one), and all of the files are added.

Stream reading is implemented if the file is larger than 1MB - if not then the file is just read into memory.

Once the zip file has been created, it is [encrypted](../dexios-core/Encryption.md). If the zip file is larger than 1mb, stream encryption will be used (unless specified manually).

We then erase the temporary zip file, using 1 pass of the [secure erase](Secure-Erase.md) feature.

### Unpacking an Archive

First, the zip file is decrypted as a temporary file with a random 8 character alphanumeric extension. This will use memory mode if it's less than 1MB, or if specified - otherwise stream mode will be used.

We then open the temporary zip file for reading, recreate the directory structure within the output directory, and extract each file to the correct path within the output directory.

Once all of the files have been successfully extracted, we securely erase the temporary zip file. This again uses 1 pass of the [secure erase](Secure-Erase.md) feature.

Zip-slip prevention measures have been taken, although it is hard to cover them all. Ensure you only unpack files from trusted sources.