# Dexios-Core

This chapter contains all of the information regarding `dexios-core`, the core library that Dexios uses.

This core library was created in order to separate the cryptographic functions away from the end-user application. Not only does this make Dexios more maintainable, but it means that the core functionality can be used in many other projects as well. It also means that the core library can expand and offer different functionality that we don't feel is a good fit for Dexios itself.

The core library handles password hashing, encryption/decryption, and management of Dexios headers. Dexios-Core also holds the `Protected` value wrapper.

You may find the `dexios-core` repo [on Github](https://github.com/brxken128/dexios-core)