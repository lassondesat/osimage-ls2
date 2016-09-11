# OS Image: Linuxstamp 2 Firmware

## Includes

* `uImage` - A image file with a U-Boot wrapper that includes the OS types and
  Boot loader information.
* `u-boot.bin` - The boot loader for the embedded system.
* `rootfs.ubi` - The root file system.
* `nandflash_yusendobc.bin` - A configured Boot Image that allows hardware
  to boot up.

**Note:** Each of these binaries are to be flashed onto the OBC board with
the `SAM-BA` application `v2.16`.

## Usage: Recovery

1.  Open `SAM-BA` application
2.  Run script – Enable `NandFlash`
3.  Run script – Scrub `Nandflash`
4.  Run script – Send Boot file `nandflash_yusendobc.bin`
5.  Select Uboot file `u-boot.bin`, load to address `0x20000`
6.  Select uImage file, load to address `0xa0000`
7.  Select filesystem `rootfs.ubi`, load to address `0x800000`
8.  Write this command into the uBoot:

    ```
    setenv bootargs console=ttyS0,115200 mtdparts=atmel_nand:8M(bootstrap/uboot/kernel)ro,-(rootfs) rw rootfstype=ubifs ubi.mtd=1 root=ubi0:rootfs
    ```

9.  Write this command into the uBoot:

    ```
    setenv bootcmd ‘nand read 0x20200000 0xA0000 0x300000;bootm 0x20200000’
    ```
