# BareMetal-newlib4

based on [BareMetal-newlib](https://github.com/ReturnInfinity/BareMetal-newlib)

Introduction
------------

This repository contains the files, script, and instructions necessary to build the [newlib](http://sourceware.org/newlib/) C library for BareMetal OS. The latest version of Newlib as of this writing is 4.4.0.20231231

newlib gives BareMetal OS access to the standard set of C library calls like `printf()`, `scanf()`, `memcpy()`, etc.

These instructions are for executing on a 64-bit Linux host. Building on a 64-bit host saves us from the steps of building a cross compiler. The latest distribution of Ubuntu was used while writing this document.


Building Details
----------------

You will need the following Linux packages. Use your prefered package manager to install them:

	libtool sed gawk bison flex m4 texinfo texi2html unzip make

You need exactly 
 * autoconf 2.69
 * automake 1.15.1

I downloaded and built them, installing them to a place on the path that overrides the installed tools

Run the build script to download, patch and build:

	./build.sh

After a lengthy compile you should have newlib installed in ./output and `test.app` built.

`./output/x86_64-pc-baremetal/lib/libc.a` is the compiled C library that is ready for linking. 
`./output/x86_64-pc-baremetal/lib/crt0.o` is the starting binary stub for your program.

`./build.sh` compiles `test.app` as

	gcc -I output/x86_64-pc-baremetal/include/ -c test.c -o test.o
	ld -T app.ld -o test.app output/x86_64-pc-baremetal/lib/crt0.o test.o output/x86_64-pc-baremetal/lib/libc.a


By default libc.a will be about 6.8 MiB. You can `strip` it to make it a little more compact. `strip` can decrease it to about 1.4 MiB.

	strip --strip-debug output/x86_64-pc-baremetal/lib/libc.a
