# Připojení sítě kabelem ethernet ručně

### síťové adaptery
- `ip -br link` vypíše názvy adaptérů
- `ip link set <adapter> up` nahodí adapter
- `ip -br link` kontrola nahození adapteru

### ip adresa
- `ip addr add 192.168.1.165/24 dev <adapter>` nastaví
- `ip -br addr` kontrola

### default gateway
- `ip route add default via 192.168.1.1`
- `ip route` kontrola

### DNS
- `echo "nameserver 192.168.1.1" > /etc/resolv.conf`

### kontrola sítě
- `ping 8.8.8.8`
- `ping archlinux.org`

---

# Připojení službou systemd-networkd

Feďo, jasně. Pro tvoji Arch laboratoř je **`systemd-networkd`** velmi pěkné řešení, protože je:

* už součástí systemd
* malé
* přehledné
* bez velkých „kouzelných“ vrstev

Ukážu ti **nejmenší funkční variantu pro Ethernet se statickou IP**.

---

# Co budeme chtít

Příklad:

* interface: `enp3s0`
* IP: `192.168.1.50/24`
* gateway: `192.168.1.1`
* DNS: `1.1.1.1`

---

# 1. Vytvoř konfigurační soubor pro interface

Soubor:

```bash
/etc/systemd/network/20-wired.network
```

Obsah:

```ini
[Match]
Name=enp3s0

[Network]
Address=192.168.1.50/24
Gateway=192.168.1.1
DNS=1.1.1.1
```

---

# 2. Zapni službu

```bash
systemctl enable --now systemd-networkd
```

---

# 3. Volitelně zapni i DNS resolver

Jestli chceš, aby DNS spravoval systemd korektně, zapni ještě:

```bash
systemctl enable --now systemd-resolved
```

a nastav `resolv.conf` jako symlink:

```bash
ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```

---

# 4. Restart služby nebo reboot

```bash
systemctl restart systemd-networkd
```

a zkontroluj:

```bash
ip addr
ip route
resolvectl status
```

---

# Výsledek

Po bootu se automaticky nastaví:

* interface se zvedne
* IP se přiřadí
* default gateway se nastaví
* DNS bude fungovat

---

# Minimal ASCII představa

```text
boot
 │
 ▼
systemd
 │
 ▼
systemd-networkd
 │
 ├─ najde enp3s0
 ├─ nastaví IP 192.168.1.50/24
 ├─ nastaví GW 192.168.1.1
 └─ nastaví DNS 1.1.1.1
```

---

# Když chceš DHCP místo statické IP

Pak je soubor ještě menší:

```ini
[Match]
Name=enp3s0

[Network]
DHCP=yes
```

To je celé.

---

# Kdy je to lepší než ruční script

Ruční script je super na pochopení:

```bash
ip link set ...
ip addr add ...
ip route add ...
```

`systemd-networkd` je další krok:

* už to přežije reboot
* konfigurace je na disku
* pořád je to jednoduché
* pořád dobře vidíš, co se děje

---

# Co bych ti doporučil teď

Na tvém minimal Archu bych šel takto:

1. nejdřív ruční `ip` příkazy
2. potom stejnou konfiguraci převést do `systemd-networkd`
3. až pak případně řešit Wi-Fi

Protože u Ethernetu krásně uvidíš princip bez rušivých věcí kolem.

---

# Malý debug tahák

Stav služby:

```bash
systemctl status systemd-networkd
```

Co networkd vidí:

```bash
networkctl
```

Detail interface:

```bash
networkctl status enp3s0
```

---

Jestli chceš, nakreslím ti hned i **Wi-Fi variantu pro `systemd-networkd` + `wpa_supplicant`**, aby sis to mohl porovnat vedle sebe.
