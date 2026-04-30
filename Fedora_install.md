# Instalace Fedora Base
---

* zdroj: *Fedora_Everything_iso*
* Instalaci je možné provádět s wifi ale po instalaci je nutné připojit kabel, protože wifi není funkční
* po instalaci:
  * `sudo dnf upgrade`
  * `sudo dnf install terminus-fonts-console`
  * `setfont ter-v24b`
  * `sudo nano /etc/vconsole.conf` a nastavit FONT="ter-v24b"
* `sudo dnf install mc -y`
* `sudo dnf install neovim bat -y`

* _dvouřádkový barevný prompt_
```bash
if [ "$EUID" -eq 0 ]; then
  PS1='\n\[\e[1;31m\]\u@\h\[\e[0m\] \[\e[1;34m\]\w\[\e[0m\]\n# '
else
  PS1='\n\[\e[1;32m\]\u@\h\[\e[0m\] \[\e[1;34m\]\w\[\e[0m\]\n$ '
fi
```
* přidat na konec souboru `/etc/bashrc`

### Instalace wifi (NetworkManager)
* nejdříve nainstalovat firmware pro wifi kartu
* `sudo dnf install iwlwifi-mvm-firmware`
* po rebootu musí být v `sudo dmesg | grep iwlwifi` vidět načtení `iwlwifi`
* `ip link` musí rovněž zobrazit wifi kartu `wlp1s0`
```bash
sudo dnf install NetworkManager NetworkManager-tui NetworkManager-wifi
sudo systemctl enable --now NetworkManager
```
* připojení k wifi příkazem `sudo nmtui`
* zjistit stav připojení `ip a` `ip link` `ip route`
* 