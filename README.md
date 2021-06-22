500Hz tick rate
EFI Stub supports
Lz4 compressed bzImage
BFQ I/O Scheduler as default
Governor performance as default
Disabled numa, kexec, debugging, etc.
AMD and Intel SoC only, disabled other SoCs
Xanmod-CacULE patchset + Gentoo patches
Enabled lz4 + z3fold zswap compressed block
Bonus?

Kurisu Makise『牧瀬 紅莉栖』 1366x768
Gentoo/Linux (as root, required pkg: cpio, lz4)
/usr/src/linux

cp .config_kurisu .config

make -j$(nproc) menuconfig

make -j$(nproc)

# Install (modules and kernel)
make -j$(nproc) modules_install
make -j$(nproc) install
Other options is compiling with LLVM toolchain.
Note! It's estimated that it may be longer than the GCC and binutils.

make LLVM=1 -j$(nproc)
If you find an area with a black background covering the console tty's font, please turn this on

CONFIG_FRAMEBUFFER_CONSOLE_DEFERRED_TAKEOVER=y
PATH:
Device Drivers -> Graphics support -> Console display driver support

Generate initramfs if using
Dracut
Adjust with the kernel version you compiled/use (as root)

dracut --kver <version> /boot/initramfs-<version>.img --force
EFI Stub example /boot vfat
(as root)

With initramfs

efibootmgr --create --part 1 --disk /dev/sda --label "GENTOO_kurisu-x86_64" --loader "\vmlinuz-5.12.5-kurisu-x86_64" \
-u "loglevel=4 initrd=\initramfs-5.12.5-kurisu-x86_64.img"
Without initramfs

efibootmgr --create --part 1 --disk /dev/sda --label "GENTOO_kurisu-x86_64" --loader "\vmlinuz-5.12.5-kurisu-x86_64" \
-u "root=PARTUUID=a157257a-6617-cd4c-b07f-2c33d4cb89f8 rootfstype=f2fs rootflags=active_logs=2,compress_algorithm=lz4 rw,noatime loglevel=4"
In order for the logo to appear on boot, make sure to use loglevel=4 in the kernel parameters.



If you want silent boot, simply use quiet instead.


