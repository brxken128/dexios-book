# **This feature has been deprecated since Version 8.4.0**

## Deprecated Notice

We have decided to remove this functionality entirely from Dexios. We did this for a few reasons:

* Pack modes cluttered up the codebase quite heavily
* Pack modes gave Dexios a larger attack-surface, with vulnerabilities such as [zip slipping](https://snyk.io/research/zip-slip-vulnerability)

It didn't feel right to leave in packing mode, but remove unpacking, so both have been deprecated. As "packing" zipped the files, you may still access your data. Just run `dexios decrypt file.enc file.zip` and open it as you would with any other zip file.

## Directory "Packing"

After using the pack feature to create an archive, it is just a zip file. You may decrypt this as normal and open it for yourself, if you so wish.

Folders encrypted with this mode have all of their information hidden. There is no way for anyone to deduce how large each file was, or their names/contents, once it has been encrypted.

This mode does not preserve metadata, file attributes or permissions (yet).

### Creating an Archive

First, the files and directory structure are both indexed. While indexing, if any files/directories match the glob pattern, they will be ignored.

The zip file is then initialised with a random 8 character alphanumeric extension, this is to prevent possible endless-loops (the buffer kept getting filled with the data that was just written).

The directory structure is recreated within the zip file (if in recursive mode, this will include other directories and not just the source one), and all of the files are added.

Stream reading is implemented if the file is larger than 1MB - if not then the file is just read into memory.

Once the zip file has been created, it is [encrypted](Encryption.md). If the zip file is larger than 1mb, stream encryption will be used (unless specified manually).

We then erase the temporary zip file, using 8 passes of the [secure erase](Secure-Erase.md) feature.

### Unpacking an Archive

First, the zip file is decrypted as a temporary file with a random 8 character alphanumeric extension. This will use memory mode if it's less than 1MB, or if specified - otherwise stream mode will be used.

We then open the temporary zip file for reading, recreate the directory structure within the output directory, and extract each file to the correct path within the output directory.

Once all of the files have been successfully extracted, we securely erase the temporary zip file. This again uses 8 passes of the [secure erase](Secure-Erase.md) feature.