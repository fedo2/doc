IWD (iNet wireless daemon) je moderní a rychlá alternativa k wpa_supplicant v Arch Linuxu pro správu Wi-Fi. Připojení se provádí pomocí příkazového řádku iwctl, kde zjistíte zařízení (device list), vyhledáte sítě (station <dev> get-networks) a připojíte se (station <dev> connect <SSID>). 

### Instalace a spuštění:
- Pokud není předinstalován, nainstalujete jej pomocí:
  `sudo pacman -S iwd`
- a aktivujete službu:
  `sudo systemctl enable --now iwd`

### Základní příkazy pro připojení k Wi-Fi:
- Spusťte interaktivní režim: `iwctl`
- Výpis Wi-Fi zařízení: [iwd]# `device list` (obvykle wlan0 nebo wlp...)
- Skenování sítí: [iwd]# `station wlan0 scan`
- Zobrazení sítí: [iwd]# `station wlan0 get-networks`
- Připojení k síti: [iwd]# `station wlan0 connect Název_Sítě`
  Pokud má síť heslo, budete vyzváni k jeho zadání. Pokud jste již byli připojeni, výzva k zadání hesla se nezobrazí

### Řešení problémů:
- Pokud device list neukazuje žádné zařízení, ujistěte se, že je Wi-Fi karta zapnutá (např. přes BIOS nebo fyzický přepínač) a že máte nainstalované příslušné ovladače.
- IWD funguje dobře samostatně nebo v kombinaci s ConnMan nebo NetworkManager. 
