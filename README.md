# makhan

Operating system written for educational purposes

## Running

### Requirements

- A Linux machine, since this was developed on Ubuntu 18.04
- Maybe a Mac will work, but I'm not sure.

### Environment setup

- Install a machine emulator like [qemu](https://www.qemu.org/download/).

#### Installing a kernel crosscompiler

(Instructions taken from [here](https://github.com/cfenollosa/os-tutorial/blob/master/11-kernel-crosscompiler/README.md))

- Install the following packages from `apt`:
  - `libgmp3-dev`
  - `libmpfr-dev libmpfr-doc libmpfr6`
  - `libmpc-dev`
  - `gcc`
  - `texinfo`
  - `lib64ncurses5-dev`

- Import the following paths:

```bash
export PREFIX="/usr/local/i386elfgcc"
export TARGET=i386-elf
export PATH="$PREFIX/bin:$PATH"
```

- Install `binutils` by following the given steps:

```bash
mkdir /tmp/src
cd /tmp/src
curl -O http://ftp.gnu.org/gnu/binutils/binutils-2.24.tar.gz # If the link 404's, look for a more recent version
tar xf binutils-2.24.tar.gz
mkdir binutils-build
cd binutils-build
../binutils-2.24/configure --target=$TARGET --enable-interwork --enable-multilib --disable-nls --disable-werror --prefix=$PREFIX 2>&1 | tee configure.log
make all install 2>&1 | tee make.log # If there is a permission denied error, use sudo
```

- Install `gcc` by following these steps:

```bash
cd /tmp/src
curl -O https://ftp.gnu.org/gnu/gcc/gcc-4.9.1/gcc-4.9.1.tar.bz2
tar xf gcc-4.9.1.tar.bz2
mkdir gcc-build
cd gcc-build
../gcc-4.9.1/configure --target=$TARGET --prefix="$PREFIX" --disable-nls --disable-libssp --enable-languages=c --without-headers
make all-gcc
make all-target-libgcc
make install-gcc
make install-target-libgcc
```

- Install `gdb` using the following instructions:

```bash
cd /tmp/src
curl -O http://ftp.rediris.es/mirror/GNU/gdb/gdb-7.8.tar.gz
tar xf gdb-7.8.tar.gz
mkdir gdb-build
cd gdb-build
export PREFIX="/usr/local/i386elfgcc"
export TARGET=i386-elf
../gdb-7.8/configure --target="$TARGET" --prefix="$PREFIX" --program-prefix=i386-elf-
make
make install
```

### Running the OS image

- To simply run the OS:

```bash
make run
```

- To run the OS in debug mode:

```bash
make debug
```

## References

- [os-tutorial](https://github.com/cfenollosa/os-tutorial)
- [OSDev](https://wiki.osdev.org/Creating_an_Operating_System)

## Notes

You can see the notes taken while working on this project in this repo: [makhanOS/notes](https://github.com/makhanOS/notes).
