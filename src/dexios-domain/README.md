# Dexios-Domain

You may find rather high-level and basic documentation on [docs.rs](https://docs.rs/dexios-domain/latest/dexios_domain/).

Dexios-Domain doesn't need much of an introduction, as it's usage is rather self explanatory. It is a layer between Dexios front-ends, and the core library.

It does offer some functionality of it's own - namely the key manipulation functions and the erase directory logic.

You craft a `Request`, and then `execute` the correct function, and your action is completed.

This library was made to allow easier integration of the Dexios format into different applications, such as a GUI front-end (or any other apps that will benefit from at-rest full file encryption).