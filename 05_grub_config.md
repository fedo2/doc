Feďo, ta chyba je normální. GRUB jen říká, že **neví jaký typ hledání má použít**, protože jsi neuvedl **typ filesystemu** nebo nemá načtený správný modul.

Stačí přidat jednu věc.

---

# 1️⃣ Správný příkaz s modulem filesystemu

Nejdřív načti modul pro ext filesystem:

`insmod ext2`

(modul `ext2` umí číst **ext2/ext3/ext4**)

Pak použij:

`search --label archroot --set=root`

Celé menuentry pak může vypadat:

```bash
menuentry "Arch test" {
    insmod ext2
    search --label archroot --set=root
    linux /boot/vmlinuz-linux root=LABEL=archroot rw
    initrd /boot/initramfs-linux.img
}
```

---

# 2️⃣ Alternativa (UUID – nejčastější v distribucích)

```bash
menuentry "Arch test" {
    insmod ext2
    search --fs-uuid XXXXX --set=root
    linux /boot/vmlinuz-linux root=UUID=XXXXX rw
    initrd /boot/initramfs-linux.img
}
```

---

# 3️⃣ Proč je potřeba `insmod ext2`

GRUB je modulární.
Bez modulu neumí číst filesystem.

Takže:

| filesystem     | modul   |
| -------------- | ------- |
| ext2/ext3/ext4 | `ext2`  |
| fat            | `fat`   |
| ntfs           | `ntfs`  |
| btrfs          | `btrfs` |

Distribuce to často načítají v hlavičce `grub.cfg`.

---

# 4️⃣ Když chceš být úplně univerzální

Distribuce často dělají:

```bash
insmod part_gpt
insmod ext2
```

protože:

* `part_gpt` → GPT partition table
* `ext2` → ext filesystem

---

# 5️⃣ Nejmenší funkční menuentry pro tvůj projekt

Já bych pro tvou flashku napsal:

```bash
menuentry "Arch Linux SSD" {
    insmod part_gpt
    insmod ext2
    search --label archroot --set=root
    linux /boot/vmlinuz-linux root=LABEL=archroot rw
    initrd /boot/initramfs-linux.img
}
```

To je **velmi robustní**.

---

# 6️⃣ Ještě jedna drobnost (která často mate)

Existují dva podobné příkazy:

| příkaz             | význam                 |
| ------------------ | ---------------------- |
| `search --label`   | hledá filesystem label |
| `search --fs-uuid` | hledá filesystem UUID  |

Ty jsi použil:

```
--fs-label
```

a to GRUB nezná.

Proto ta chyba.

---

Feďo, a ještě jedna věc, která by tě mohla nadchnout:

GRUB má příkaz **`probe`**, kterým můžeš zjistit:

* filesystem
* UUID
* label

přímo v GRUB shellu.

Takže si můžeš v bootloaderu udělat **diagnostiku disků bez Linuxu**. Je to docela cool nástroj při ladění bootu.
