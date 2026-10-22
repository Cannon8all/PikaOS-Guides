# Step 1: Identify Your Partitions
**Open a terminal in your live ISO environment. Run the following command to list all available drives and partitions:**

>sudo lsblk -f

**Identify and write down your specific partition names:**

- Root partition (/): Typically a large Btrfs or EXT4 partition.
- Boot partition (/boot): The partition holding the Linux kernel.
- EFI partition: A small FAT32 formatted partition.

# Step 2: Mount the Installed System
**Replace the example partition names (nvme0n1pX) with the actual Root partition (/) partition found in Step 1.**
## Option A: For Btrfs Filesystems

>sudo mkdir -p /mnt
>sudo mount -o subvol=@ /dev/nvme0n1p3 /mnt


## Option B: For Non-Btrfs Filesystems (XFS, EXT4)
**If you did not use Btrfs during installation, remove the subvolume flag:**

>sudo mkdir -p /mnt
>sudo mount /dev/nvme0n1p3 /mnt

**Mount the Boot and EFI Partitions
bash**

>sudo mount /dev/nvme0n1p2 /mnt/boot
>sudo mount /dev/nvme0n1p1 /mnt/boot/efi


# Step 3: Mount Virtual Filesystems
**Bind the host system runtime directories to the target mount point:**

>sudo mount --types proc /proc /mnt/proc
>sudo mount --rbind /sys /mnt/sys
>sudo mount --rbind /dev /mnt/dev
>sudo mount --rbind /run /mnt/run


**Set the propagation state to slave to prevent host environment lockups:**

>sudo mount --make-rslave /mnt/sys
>sudo mount --make-rslave /mnt/dev
>sudo mount --make-rslave /mnt/run


# Step 4: Enter the Chroot Environment
**Execute the chroot command to log into your installed system as the root user:**

>sudo chroot /mnt


> **You can now run commands, fix updates, or repair the bootloader directly on your installed operating system.**
{.is-success}
# Step 5: Safe Exit and Cleanup
**Once repairs are complete, exit the chroot environment and cleanly unmount the partitions before rebooting:**

>exit
>sudo umount -R /mnt
>sudo reboot