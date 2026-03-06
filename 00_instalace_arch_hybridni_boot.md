Feďo, připravil jsem **maximálně minimalistický cheat-sheet**, který můžeš mít klidně uložený v repozitáři nebo vytisknutý vedle notebooku. Je to **celá instalace Archu na externí SSD s BIOS + UEFI bootem** v asi 20 příkazech.

Je to styl, který se dobře hodí pro tvoji **experimentální Arch laboratoř**.

---

# Arch Linux – hybrid instalace (BIOS + UEFI)

## 0️⃣ Identifikace disku

```bash
lsblk
```

příklad:

```
/dev/sdb   ← externí SSD
```

---

# 1️⃣ Rozdělení disku

GPT + BIOS boot + EFI + ROOT

```bash
sgdisk -Z /dev/sdb

sgdisk -n1:0:+1M   -t1:ef02 /dev/sdb
sgdisk -n2:0:+512M -t2:ef00 /dev/sdb
sgdisk -n3:0:0     -t3:8300 /dev/sdb
```

| oddíl | účel      |
| ----- | --------- |
| sdb1  | BIOS boot |
| sdb2  | EFI       |
| sdb3  | root      |

---

# 2️⃣ Filesystem

```bash
mkfs.fat -F32 /dev/sdb2
mkfs.ext4 /dev/sdb3
```

---

# 3️⃣ Mount

```bash
mount /dev/sdb3 /mnt

mkdir /mnt/boot
mkdir /mnt/boot/efi

mount /dev/sdb2 /mnt/boot/efi
```

---

# 4️⃣ Instalace systému

```bash
pacstrap /mnt base linux linux-firmware grub
```

doporučeno:

```bash
pacstrap /mnt base linux linux-firmware grub vim git
```

---

# 5️⃣ fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

---

# 6️⃣ chroot

```bash
arch-chroot /mnt
```

---

# 7️⃣ čas

```bash
ln -sf /usr/share/zoneinfo/Europe/Prague /etc/localtime
hwclock --systohc
```

---

# 8️⃣ locale

```bash
nvim /etc/locale.gen
```

odkomentuj

```
en_US.UTF-8 UTF-8
cs_CZ.UTF-8 UTF-8
```

pak:

```bash
locale-gen
```

---

# 9️⃣ GRUB BIOS

```bash
grub-install --target=i386-pc /dev/sdb
```

---

# 🔟 GRUB UEFI

```bash
grub-install \
--target=x86_64-efi \
--efi-directory=/boot/efi \
--bootloader-id=ARCH \
--removable
```

---

# 1️⃣1️⃣ GRUB config

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

---

# 1️⃣2️⃣ root heslo

```bash
passwd
```

---

# 1️⃣3️⃣ reboot

```bash
exit
umount -R /mnt
reboot
```

---

# Výsledek

Disk bootuje:

| režim        | jak                        |
| ------------ | -------------------------- |
| BIOS         | GRUB z BIOS boot partition |
| UEFI         | EFI/BOOT/BOOTX64.EFI       |
| libovolný PC | díky `--removable`         |

---

# Velmi dobrý další krok pro tvůj projekt

Jak tě znám, bude tě bavit **postupně systém stavět**.

Já bych šel:

```
Phase 1
base + grub

Phase 2
network + ssh

Phase 3
tmux + nvim + git

Phase 4
python + node

Phase 5
dev environment
```

A máš z toho krásný **učební Arch notebook**.

---

Feďo, ještě ti můžu ukázat jednu **velmi geek věc**, která se ti bude líbit:

👉 **Jak udělat Arch instalaci jen asi 120 MB velkou**

(skoro jako staré Slackware z CD)

a přitom plně funkční. To je u Archu docela krásná disciplína.
