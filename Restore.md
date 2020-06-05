After installing the grub2 into MBR, the boot may fail and stuck with `No TPM found error`. One may need to restore the bootloader back to sane.

For Fedora, the [reference](https://docs.pagure.org/docs-fedora/the-grub2-bootloader.html) has detailed steps, under `Restoring the bootloader using the Live disk.`

Steps copy-pasted below:

**Procedure**
1. Boot the Fedora live system from the bootable device you have created.

2. Open the terminal.

3. Examine the partition layout and identify the boot and the root partition.
```
$ sudo fdisk -l
```
If you are using the default Fedora layout, there will be one /dev/sda1 partition that holds the /boot directory and one /dev/mapper/fedora-root that holds the root file system.

4. Create the mount point for the root partition.

```
$ sudo mkdir -p /mnt/root
```

5. Mount the root partition on the mount point.

```
$ sudo mount /dev/mapper/fedora-root /mnt/root
```

6. Mount the boot partition in the boot directory of the filesystem that you have mounted in the previous step.

```
$ sudo mount /dev/sda1 /mnt/root/boot/
```

7. Mount system processes and devices into the root filesystem in /mnt/root.

```
$ sudo mount -o bind /dev /mnt/root/dev
$ sudo mount -o bind /proc /mnt/root/proc
$ sudo mount -o bind /sys /mnt/root/sys
$ sudo mount -o bind /run /mnt/root/run
```

8. Change your filesystem into the one mounted under /mnt/root.
```
$ sudo chroot /mnt/root
```

9. Reinstall GRUB2 into the MBR of the primary hard disk.
```
$ sudo grub2-install --no-floppy --recheck /dev/sda
```

10. Recreate the GRUB2 configuration files.

```
$ sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

11. Exit this temporary root filesystem.

```
$ exit
```

12. Your bootloader should be now restored. Reboot your computer to boot into your normal system.
```
$ sudo systemctl reboot
```
