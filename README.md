# GLIBC compatibility header

This project provides a single header file `glibc-compat.h` that contains versioned symbols from an old version of GLIBC ([2.17](https://sourceware.org/legacy-ml/libc-announce/2012/msg00001.html)). When using this header the compiler is directed to use the old versions of the GLIBC symbols and because GLIBC is backwards compatible the resulting program will run on practically any modern Linux distribution (that is GLIBC-based).

The easiest way to use the `glibc-compat.h` header is by including the `-include glibc-compat.h` GCC option when building your project. For example, you may include this option in your `CFLAGS`, etc.

## Caveats

When using `glibc-compat.h` you should be aware of the following caveats:

- You might miss important bug fixes in newer symbols.

- It is likely that multiple versions of the same symbol might be used when dyna-linking with other libs. For example, `fprintf@GLIBC_2.x` might be used by the main executable and `fprintf@GLIBC_2.y` might be used by a `dlopen`'ed library. I have seen claims on the Internet that this *should* work, but nothing authoritative.

- It is possible that using old symbols with new headers can also cause problems. For example, suppose that a new header changed the definition of an internal `struct` that is accessed via a macro or inline from the header file. We could then have code in the executable using both the new `struct` definition via the macro and the old `struct` definition via the old symbol reference. (I am not aware of any such cases, but it is a possibility.)

- This approach will build executables that run on glibc-based systems. Distros that uses other libc's (e.g. Alpine/MUSL) are not supported.

## License

The License gives you all the freedoms of the MIT License, except that you may NOT remove the License notice from any of the files.
