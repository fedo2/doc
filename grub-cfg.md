```bash
set default=0
set timeout=10

set DISKLABEL=archroot
set KERNEL=/boot/vmlinuz-linux
set INITRD=/boot/initramfs-linux.img

menuentry "Arch Linux SSD" {
    insmod part_gpt
    insmod ext2
    search --label $DISKLABEL --set=root
    linux $KERNEL root=LABEL=$DISKLABEL rw
    initrd $INITRD
}
```

Partition musí mít nastavený `label`, který na nastaví buď při formátování partition:
`mkfs.ext4 -L archroot /dev/sdb3`
nebo dodatečně příkazem:
`e2label /dev/sdb3 archroot`
