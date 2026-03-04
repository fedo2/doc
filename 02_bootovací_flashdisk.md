### Vytvoření externího bootovacího disku pro BIOS i UEFI

##### Rozdělení disku

Struktura disku bude GPT, sector 512 byte

- sector 0:        MBR boot.img
  sector 1 - 2047: partition entry, 1 MiB místo vytvořeno automaticky (většinou)
  sector 2048... : sdX1, 1 MiB pro core.img, neformátované
  další oblast... : sdX2, 512 MiB pro EFI, format FAT32
  další oblast... : sdX3: zbytek disku pro ROOT, format ext4

- Zjistíme skutečné jméno disku příkazem `lsblk`
pokud disk není prázdný a má nějaké oddíly, vymažeme jej `sudo wipefs -a /dev/sdX`

- Disk rozdělíme příkazem `cfdisk` a u prázdného disku se po spuštění zobrazí tabulka s výběrem typu disku. Vybereme `GPT`.

při vytváření používáme volbu `new`, `type` a na závěr `write`

- Typy oddílů:
sdX1 `BIOS boot`
sdX2 `EFI system`
sdX3 `Linux filesystem`

##### Formátování

- `mkfs.fat -F32 /dev/sdX2`
- `mkfs.ext4 /dev/sdX3`

##### Připojení ESP

`mount /dev/dsX2 /mnt`

##### Instalace GRUB BIOS

`grub-install --target=i386-pc --boot-directory=/mnt/boot --recheck /dev/deX`

##### Instalace GRUB UEFI

`grub-install --target=x86_64-efi --efi-directory=/mnt --boot-directory=/mnt/boot --removable --recheck`

##### Vytvoření společného grub.cfg

`mkdir -p /mnt/boot/grub`
`touch /mnt/boot/grub/grub.cfg`
`nano /mnt/boot/grub/grub.cfg`

```ini
set timeout=10
set default=0

menuentry "Hybrid USB (UEFI + BIOS) – test GRUB" {
    echo "GRUB nabehl a nacetl grub.cfg."
    echo "Pokud jsi sem dosel, funguje zavadeni."
    echo "UEFI i BIOS pouzivaji stejny grub.cfg."
    read
}
```
##### Odpojení

`sync`
`umount /mnt`

##### Kontrola oddílů

`lsblk -f /dev/sdX`