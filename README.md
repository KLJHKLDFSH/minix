# MINIX 3

MINIX 3 is a free, open-source, Unix-like operating system designed to be highly reliable, flexible, and secure. It is based on a tiny microkernel running in kernel mode with the rest of the OS running as a collection of isolated, protected processes in user mode.

This repository contains the source code for **MINIX 3.4.0**, automatically replicated from [gerrit.minix3.org](https://gerrit.minix3.org).

## Features

- **Microkernel architecture** – the kernel is very small; device drivers, file systems, and servers all run in user space as separate processes
- **Self-healing** – device drivers and servers can be automatically restarted if they crash, without taking down the whole system
- **POSIX-compatible** – supports a large subset of the POSIX API
- **NetBSD userland** – the user-space utilities are synchronized with NetBSD 8
- **Supported architectures**: x86 (i386) and ARM (earm)

## Repository Structure

```
.
├── bin/            Basic user-space utilities (cat, cp, sh, …)
├── crypto/         Cryptographic libraries and tools (OpenSSL, Heimdal, …)
├── dist/           Miscellaneous distribution files
├── docs/           Documentation (UPDATING, profiling notes)
├── etc/            System configuration files
├── external/       Third-party software (LLVM/Clang, flex, …)
├── games/          Classic Unix games
├── gnu/            GNU tools (grep, tar, …)
├── include/        System header files
├── lib/            System libraries (libc, libm, …)
├── libexec/        System executables not in PATH (ftpd, …)
├── minix/          MINIX-specific source code
│   ├── drivers/    Device drivers (storage, net, tty, USB, …)
│   ├── fs/         File systems (MFS, ext2, ISO 9660, procfs, …)
│   ├── include/    MINIX-specific headers
│   ├── kernel/     Microkernel (arch/i386, arch/earm)
│   ├── lib/        MINIX libraries
│   ├── net/        Networking stack
│   ├── servers/    System servers (PM, VFS, VM, RS, DS, …)
│   └── tests/      MINIX-specific test suite
├── releasetools/   Scripts for building release images
├── sbin/           System administration utilities
├── sys/            Machine-independent kernel interfaces
├── tests/          ATF-based test suite
├── tools/          Host build tools
├── usr.bin/        Additional user-space utilities
└── usr.sbin/       Additional system administration utilities
```

### Key MINIX Servers

| Server | Description |
|--------|-------------|
| `pm`   | Process Manager – handles `fork`, `exec`, signals, and accounting |
| `vfs`  | Virtual File System – routes file-system requests to the appropriate FS server |
| `vm`   | Virtual Memory – manages address spaces and paging |
| `rs`   | Reincarnation Server – monitors and restarts failed system services |
| `ds`   | Data Store – publish/subscribe key-value store for system components |
| `sched`| Scheduler – implements the CPU scheduling policy |
| `mib`  | MIB server – provides `sysctl(3)` information to user space |
| `is`   | Information Server – returns system information for debugging |

## Building MINIX

MINIX uses the `build.sh` wrapper script (derived from NetBSD) to drive the build.

### Prerequisites

- A cross-compilation toolchain is built automatically by `build.sh`
- A POSIX-compatible shell (`/bin/sh`, bash, ksh, …)
- GNU `make` or a compatible `make`

### Quick Start

```sh
# Build the cross-compile tools
./build.sh -m i386 -j4 tools

# Build the full OS
./build.sh -m i386 -j4 -U distribution

# Build a bootable x86 disk image (target name selects the image format)
./build.sh -m i386 -j4 disk-image=hdimage
```

Replace `i386` with `earm` to target ARM.

### Common `build.sh` Options

| Option | Meaning |
|--------|---------|
| `-m mach` | Target machine type (`i386`, `earm`) |
| `-j njob` | Parallel jobs |
| `-U` | Unprivileged build (no root required) |
| `-u` | Incremental build (skip `cleandir`) |
| `-D dest` | Staging directory (default: `destdir.MACHINE`) |
| `-R release` | Release directory (default: `releasedir`) |
| `-T tools` | Tool directory (default: `tooldir.MACHINE`) |

### Build Operations

| Operation | Description |
|-----------|-------------|
| `tools` | Build and install host cross-tools |
| `build` | Build the entire system |
| `distribution` | Build + install configuration files |
| `release` | Full release build including kernels |
| `kernel=conf` | Build a single kernel with the given config |
| `sets` | Create binary distribution sets |
| `iso-image` | Create a bootable CD-ROM image |
| `disk-image=target` | Create a bootable disk image |

## License

MINIX 3 is distributed under a BSD-style license. See [LICENSE](LICENSE) for details.

## Links

- Official website: <https://www.minix3.org>
- Official source mirror: <https://gerrit.minix3.org>
- MINIX 3 wiki: <https://wiki.minix3.org>
- Mailing lists: <https://www.minix3.org/lists.html>
