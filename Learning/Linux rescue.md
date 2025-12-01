# Linux Rescue Guide (Systemd‑Boot + No USB)

This document explains how to repair, reinstall, and recover Linux **without a USB**, using your **10GB CachyOS rescue installation** and **systemd‑boot**. Designed for catastrophic situations such as broken systems, overwritten bootloaders, failed updates, or partition mistakes.

---

## ❗ When to Use This Guide

Use this guide if:

- Linux won’t boot
    
- Bootloader disappeared after a Windows update
    
- Desktop environment is broken
    
- System became unusable after experimenting
    
- You want a full reinstall without using a USB drive
    

---

## 🔥 Important Safety Notes

- **Never shrink or modify Windows partitions from Linux.** Do it from Windows only.
    
- **Never run this guide from your main broken Linux.** Always boot into the rescue 10GB CachyOS.
    
- **systemd‑boot is your bootloader — not GRUB.** All commands follow systemd‑boot.
    

---

## 🧠 Boot into the Rescue System

1. Power on the laptop
    
2. At boot menu, choose the entry for the **10GB rescue CachyOS** partition
    
3. Log in normally
    

You are now operating from the recovery environment.

---

## 🧩 Identify Partitions

Useful commands:

```
lsblk
blkid
```

|Type|What it contains|
|---|---|
|Rescue partition|Boots into 10GB CachyOS|
|Main root partition|The broken Linux system|
|EFI partition|Contains systemd‑boot loaders|

---

## 🔥 A — Repair Bootloader When Linux Won’t Boot

### Symptoms

- Linux not shown in BIOS boot
    
- Boots straight into Windows
    
- Boot entry missing
    

### Steps

1. Boot into the 10GB rescue CachyOS
    
2. Mount the main Linux root partition:
    

```
sudo mount /dev/nvme0n1pX /mnt
```

3. Mount EFI if it is separate:
    

```
sudo mount /dev/nvme0n1pY /mnt/boot
```

4. Chroot into the broken system:
    

```
sudo arch-chroot /mnt
```

5. Reinstall systemd‑boot:
    

```
bootctl install
```

6. Update boot entries:
    

```
bootctl update
```

7. Exit and reboot.
    

```
exit
reboot
```

Linux should appear in the boot menu again.

---

## 🔥 B — Repair a Broken Linux Installation (without reinstall)

### Use this when

- You updated kernel and it broke
    
- Desktop environment got corrupted
    
- Package conflicts broke login
    

### Steps

1. Boot into rescue CachyOS
    
2. Mount and chroot:
    

```
sudo mount /dev/nvme0n1pX /mnt
sudo mount /dev/nvme0n1pY /mnt/boot
sudo arch-chroot /mnt
```

3. Fix packages / kernel / desktop  
    Examples:
    

```
pacman -S linux linux-headers
pacman -S hyprland plasma xfce gnome  (if reinstalling desktop)
pacman -Syu
```

4. Exit and reboot.
    

```
exit
reboot
```

System returns to normal without a full reinstall.

---

## 🔥 C — Full Reinstall of Linux Without USB

### Use this when

- System is beyond repair
    
- You want a completely fresh install
    

### Steps

1. Boot into rescue CachyOS
    
2. Format only the main Linux partition:
    

```
sudo mkfs.ext4 /dev/nvme0n1pX
```

3. Mount it:
    

```
sudo mount /dev/nvme0n1pX /mnt
```

4. Install base Linux system:
    

```
sudo pacstrap /mnt base linux linux-firmware
```

5. Generate fstab:
    

```
sudo genfstab -U /mnt | sudo tee /mnt/etc/fstab
```

6. Chroot:
    

```
sudo arch-chroot /mnt
```

7. Install bootloader:
    

```
bootctl install
bootctl update
```

8. Install desktop environment, apps, and drivers.
    
9. Exit and reboot.
    

```
exit
reboot
```

Fresh Linux installed **without any USB drive**.

---

## 💣 Useful Mounting Reference

```
/mnt              → main linux root
/mnt/boot         → EFI partition
/usr/lib/kernel/install.d → systemd‑boot kernel hooks
/boot/loader/entries/*.conf → individual boot entries
```

To edit a boot entry manually:

```
sudo nano /boot/loader/entries/*.conf
```

Update after editing:

```
bootctl update
```

---

## ⚡ Recommended Tools to Pre‑Install on the Rescue System

|Purpose|Tool|
|---|---|
|Partitioning|gparted|
|Boot fixing|systemd‑boot tools (already present)|
|Kernel repair|pacman|
|Disk analysis|nvme-cli|
|Data recovery|testdisk / photorec|
|Btrfs snapshots (optional)|btrfs-assistant|

Having these pre-installed makes the rescue partition unstoppable.

---

## 🏁 Final Notes

- The 10GB rescue CachyOS you keep is **the best protection you can have**.
    
- With systemd‑boot, recovery is far easier than GRUB.
    
- No USB will ever be required if this guide is followed.
    

If Linux dies — **this guide makes sure Linux resurrects Linux.**

---

**End of Linux Rescue Guide**