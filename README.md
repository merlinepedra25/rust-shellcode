# Rust Shellcode
A template project for writing shellcode in rust.

The goal of this template is to make it easy to write shellcode for the major architectures in rust without a lot of custom configuration.

One of the drawbacks of this template is that the shellcode may not compile down to the smallest possible size as handmade or something
more targeted to a specific architecture. x86_64 and aarch64 optimize to be quite small, but the others can get a little bloated.

## Getting Started
### Binutils
To use this template, a copy of binutils for the target architecture must be installed. Compiling binutils from scratch is quite easy
and the recommended method.

Go to `https://mirrors.ocf.berkeley.edu/gnu/binutils/` and download the newest version of binutils (NOTE: This template was made using binutils version 2.38).
Extract the tar archive.

Run:
```bash
./configure --prefix=[your install prefix here] \
            --target=[target triple here] \
            --disable-static \
            --disable-multilib \
            --disable--werror \
            --disable-nls

make

make install
```

You should use one of the following target triples:
 - x86_64-unknown-linux-gnu
 - i386-unknown-linux-gnu
 - aarch64-unknown-linux-gnu
 - arm-unknown-linux-gnu
 - mipsel-unknown-linux-gnu
 - mips64el-unknown-linux-gnu

### Configuration
The `Makefile` will need to be configured to build for the proper architecture. You can grep for 'FIXME' to
see what to change. The only things that should be changed are the `CROSS_BINUTILS` variable and the `TARGET` variable.

`CROSS_BINUTILS` should be set the where binutils was compiled or installed to. `TARGET` should be the target triple of
the architecture to compile for. See `.cargo/config.toml` for the available targets.

### Building
Just run `make`. `shellcode.bin` will be built if there are no errors. I recommend running `objdump` from binutils on the binary just to
sanity check the output. `make dis` will run objdump on the elf file generated by rustc with the debug information not stripped. That should
match exactly the assembly in `shellcode.bin`. I have done limited testing of this template so ymmv.

## Architectures
The currently supported architectures are:
 - x86_64-unknown-linux-gnu
 - i686-unknown-linux-gnu
 - armv7a-none-eabi
 - aarch64-unknown-none
 - mipsel-unknown-linux-musl
 - mips64el-unknown-linux-muslabi64

It should be pretty easy to add support for other mips or arm targets. Just copy the target configuration and add a new target triple in the
`.cargo/config.toml` file. It might require some configuration of linker flags or change to linker script but probably nothing too complicated.

## Contributing
I'm happy to accept PRs to add new architectures or to help with code size.
