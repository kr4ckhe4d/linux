# Linux Kernel Directory Structure

## Core Kernel Components
- **kernel/** - Core kernel functionality (scheduler, process management, timers, signals)
- **mm/** - Memory management (virtual memory, page allocation, swapping)
- **fs/** - File systems (VFS layer, ext4, btrfs, proc, sysfs, etc.)
- **net/** - Networking stack (TCP/IP, sockets, protocols)
- **ipc/** - Inter-process communication (shared memory, semaphores, message queues)
- **init/** - Kernel initialization code

## Hardware & Architecture
- **arch/** - Architecture-specific code (x86, ARM, RISC-V, etc.)
- **drivers/** - Device drivers (USB, PCI, graphics, storage, network cards)
- **sound/** - Audio subsystem and sound drivers

## Security & I/O
- **security/** - Security frameworks (SELinux, AppArmor, capabilities)
- **crypto/** - Cryptographic algorithms and frameworks
- **block/** - Block device layer (hard drives, SSDs)
- **io_uring/** - High-performance async I/O interface

## Development & Build
- **include/** - Header files for kernel APIs
- **lib/** - Library functions used throughout kernel
- **scripts/** - Build scripts, configuration tools, static analysis
- **tools/** - Userspace tools for kernel development/testing
- **samples/** - Example code and kernel modules

## Documentation & Misc
- **Documentation/** - Kernel documentation (start with process/changes.rst)
- **LICENSES/** - License files
- **certs/** - Certificates for module signing
- **usr/** - Built-in initramfs
- **virt/** - Virtualization support (KVM)
- **rust/** - Rust language support in kernel

## Getting Started
1. Read `Documentation/process/changes.rst` for build requirements
2. Use `make htmldocs` or `make pdfdocs` to build documentation
3. Online docs: https://www.kernel.org/doc/html/latest/
