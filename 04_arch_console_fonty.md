### Nastavení většího fontu v konzoli terminálu

fonty jsou uloženy v `/usr/share/kbd/consolefonts`

konfigurace pro konzoli je v `/etc/vconsole.conf`

stáhněte font:
`pacman -S terminus-font`

okamžité nastavení fontu (do restartu):
`setfont ter-v22b` nastaví terminus, 22px, bold

trvalé nastavení `/etc/vconsole.conf`:
```sh
KEYMAP=cz-quertz
FONT=ter-v22b
...
```
aktivace:
`systemctl restart systemd-vconsole-setup.service`
