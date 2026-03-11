# Arch Linux, zprovoznění sítě.

balíček pro práci se sítí:
`iproute2` obsahuje příkaz `ip` a je předinstalován

---

# 🧠 Minimum balíčků pro síť v Archu

Já osobně používám tento **minimal set**:

```bash
pacman -S iproute2 iw wpa_supplicant dhcpcd
```

co dělají:

| balíček          | funkce                          |
| ---------------- | ------------------------------- |
| `iproute2`       | příkaz `ip` (rozhraní, routing) |
| `iw`             | práce s WiFi kartou             |
| `wpa_supplicant` | WPA/WPA2 autentizace            |
| `dhcpcd`         | získání IP z DHCP               |

To je **nejmenší realistický stack**.

(Alternativa je `iwd`, ale ta je paradoxně trochu komplexnější.)

---

# 🌐 Cheatsheet – Ethernet (nejjednodušší)

Ethernet je krásně jednoduchý.

### 1️⃣ zapnout interface

```bash
ip link
```

např.

```
eth0
enp3s0
```

pak

```bash
ip link set enp3s0 up
```

---

### 2️⃣ získat IP

```bash
dhcpcd enp3s0
```

---

### 3️⃣ test

```bash
ping archlinux.org
```

Hotovo.

---

# 📶 Cheatsheet – WiFi (minimum)

### 1️⃣ zapnout interface

```bash
ip link set wlp3s0 up
```

---

### 2️⃣ zjistit dostupné sítě

```bash
iw dev wlp3s0 scan | grep SSID
```

---

### 3️⃣ vytvořit config

```bash
wpa_passphrase MOJE_WIFI HESLO > wifi.conf
```

---

### 4️⃣ připojit WiFi

```bash
wpa_supplicant -B -i wlp3s0 -c wifi.conf
```

---

### 5️⃣ získat IP

```bash
dhcpcd wlp3s0
```

---

# ⚡ Ultra minimalistická verze (script)

```bash
ip link set wlp3s0 up
wpa_passphrase MOJE_WIFI HESLO > wifi.conf
wpa_supplicant -B -i wlp3s0 -c wifi.conf
dhcpcd wlp3s0
```

---

# 🧠 Pro tvoji Arch laboratoř

Doporučil bych ti udělat **profil minimálního systému**:

### absolutní minimum balíčků

```
linux
bash
coreutils
iproute2
iw
wpa_supplicant
dhcpcd
```

a z toho už postavíš skoro všechno.

---

# ⚙️ Geek poznámka (důležitá)

WiFi ve skutečnosti potřebuje **4 vrstvy**:

```
kernel driver (ath9k)
↓
nl80211 subsystem
↓
wpa_supplicant
↓
DHCP client
```

Ethernet potřebuje jen:

```
kernel driver
↓
DHCP client
```

Proto je **wifi vždy o řád složitější**.

---

Jestli chceš, můžeme ještě udělat jednu věc, která by tě podle mě bavila:

**udělat Arch instalaci která má jen 12 balíčků a přesto funguje síť.**

Je to docela pěkné cvičení na pochopení Linux userspace.
