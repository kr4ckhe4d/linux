# Raspberry Pi Kernel Compilation and Deployment Guide

### 1. Install Dependencies

Install the required build tools and cross-compiler from the Arch repositories:

```bash
sudo pacman -Syu base-devel git bc aarch64-linux-gnu-gcc
```

### 2. Clone Kernel Source

Get the official Raspberry Pi Linux kernel. Using `--depth=1` saves time and disk space:

```bash
git clone --depth=1 https://github.com/raspberrypi/linux
cd linux
```

### 3. Configure the Kernel

The Pi Zero 2 W uses the `bcm2711` configuration for 64-bit builds (same as Pi 3/4):

```bash
# 1. Load default config
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- bcm2711_defconfig

# 2. (Optional) Customize settings/drivers
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- menuconfig
```

### 4. Modify Source Code (The "Hello World" Hack)

Add a custom print statement that runs immediately when the kernel starts.

1. Open `init/main.c`

2. Search for `start_kernel(void)`

3. Add your `pr_info` lines at the very top of the function:

   ```c
   asmlinkage __visible void __init start_kernel(void)
   {
       char *command_line;
       char *after_dashes;
    
       /* --- CUSTOM CODE START --- */
       pr_info("###################################################\n");
       pr_info("CACHYOS USER WAS HERE: Kernel is booting up!\n");
       pr_info("###################################################\n");
       /* --- CUSTOM CODE END --- */
    
       set_task_stack_end_magic(&init_task);
       // ... rest of function
   }
   ```
### 5. Compile

Build the kernel image, modules, and device tree blobs:

```bash
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- Image modules dtbs -j$(nproc)
```

### 6. Install to SD Card

**Prerequisites:** Mount your SD card and ensure paths match your actual mount points.

- Replace `nipuna` with your username
- Verify your SD card mount points before proceeding
    

#### A. Install Modules (RootFS)

Install drivers to the root partition (`rootfs`):

```bash
sudo make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- \
INSTALL_MOD_PATH=/run/media/nipuna/rootfs modules_install
```

#### B. Install Kernel Image (BootFS)

Back up the old kernel and copy the new one. The 64-bit kernel file is `kernel8.img`:

```bash
# Backup
sudo cp /run/media/nipuna/bootfs/kernel8.img /run/media/nipuna/bootfs/kernel8-backup.img

# Install
sudo cp arch/arm64/boot/Image /run/media/nipuna/bootfs/kernel8.img
```

#### C. Install Device Trees (BootFS)

Copy the hardware definitions required for boot:

```bash
# Basic DTBs
sudo cp arch/arm64/boot/dts/broadcom/*.dtb /run/media/nipuna/bootfs/

# Overlays
sudo cp arch/arm64/boot/dts/overlays/*.dtb* /run/media/nipuna/bootfs/overlays/
sudo cp arch/arm64/boot/dts/overlays/README /run/media/nipuna/bootfs/overlays/
```

### 7. Verify

Unmount safely, insert into Pi, and check the logs:

```bash
dmesg | grep "CACHYOS"
```