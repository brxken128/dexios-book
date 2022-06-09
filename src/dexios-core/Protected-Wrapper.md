## Protected Wrapper

We have implemented our own `Protected<>` wrapper/type, that implements zeroize-on-drop. This ensures all sensitive information is securely zeroed-out once it is no longer required, and that the sensitive informatiton is only accessed when explicit calls are made. Our implementation was inspired by the `secrecy` crate's functionality, so I'd like to provide a huge thanks to the original creators.

Protected values do not implement any copying/cloning, and there are none done manually within the codebase. This is to prevent possible leaks of data.

Protected values are also redcated from debug logs and such, and they will show "[REDACTED]" whenever they are printed. They do not implement `fmt::display`.