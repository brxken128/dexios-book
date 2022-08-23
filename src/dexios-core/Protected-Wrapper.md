## Protected Wrapper

We have implemented our own `Protected<>` wrapper/type, that implements zeroize-on-drop. This ensures all sensitive information is securely zeroed-out once it is no longer required, and that the sensitive informatiton is only accessed when explicit calls are made. Our implementation was inspired by the `secrecy` crate's functionality, so I'd like to provide a huge thanks to the original creators.

Protected values do not implement any copying. This is to prevent possible leaks of data.

Protected values may be explicitly cloned, but this requires `.clone()` or similar - you can easily find these within a codebase.

The only way to access the information held within this type, is to call `.expose()` - this makes things very manageable. With this, we are able to quickly and easily see wherever protected information is accessed. Not only does this allow us to detect potentially malicious pull requests and such, but it also makes [auditing](Auditing.md) a lot easier.

Protected values are also redcated from debug logs and such, and they will show "[REDACTED]" whenever they are printed. They do not implement `fmt::display`.