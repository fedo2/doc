# Nastavení jasu displeje

V Arch Linuxu je jas displeje řízený přes **kernel rozhraní v `/sys/class/backlight`**. Je dobré si to ukázat přímo na tom nízko-úrovňovém principu, protože přesně zapadá do tvého stylu „pochopit jak to funguje uvnitř“.

---

## 1️⃣ Zjisti, jaký driver ovládá podsvícení

```markdown
ls /sys/class/backlight
```

Typicky uvidíš něco jako:

```markdown
intel_backlight
acpi_video0
amdgpu_bl0
```

Na tvém **Aceru s Intel grafikou** to velmi pravděpodobně bude:

```markdown
intel_backlight
```

---

## 2️⃣ Podívej se na aktuální hodnoty

Například:

---

## 3️⃣ Nastav vyšší jas

Musíš být **root**, protože zapisuješ do kernelového rozhraní.

Například:

nebo jako root:

---

## 4️⃣ Praktický test

Postupně:

Uvidíš okamžitou změnu.

---

## 5️⃣ Proč někdy nejde zvýšit jas

Často jsou dva důvody:

### a) používá se špatný driver

Např.:

```markdown
acpi_video0
```

má menší rozsah než

```markdown
intel_backlight
```

Pak je potřeba používat **intel\_backlight**.

---

### b) BIOS limit

Některé notebooky nastavují limit.

Pak pomůže kernel parametr:

```markdown
acpi_backlight=vendor
```

nebo

```markdown
acpi_backlight=native
```

(v GRUB).

---

## 6️⃣ Malý trik (pohodlné ovládání)

Můžeš si udělat alias:

nebo malý skript.

---

## 7️⃣ Když chceš „linuxově správný“ nástroj

Balík:

pak:

---

💡 **Tip pro tvůj Arch projekt**

Tenhle `/sys` mechanismus je krásná ukázka filozofie Linuxu:

Je to vlastně **"everything is a file"** v praxi.

---

Jestli chceš, můžeme ještě udělat jednu zajímavou věc:  
ukážu ti **proč někdy Fn klávesy na jas nefungují** a jak to Arch řeší (to je pěkná detektivka s `acpi_listen` a `udev`). To je přesně věc, na kterou člověk u starších Acerů často narazí.

Feďo, dobrá otázka – tady se krásně ukáže rozdíl mezi **aliasem a funkcí v bash**.

## 1️⃣ Alias parametry neumí

Alias je jen **textová náhrada příkazu**. Bash ho pouze rozepíše.

Například:

když napíšeš

```markdown
ll /etc
```

bash z toho udělá:

Ale alias **neumí pracovat s `$1`, `$2` apod.**

Například toto **nebude fungovat správně**:

protože `$1` se nikdy nenahradí.

---

## 2️⃣ Na parametry se používá funkce

Takže správná varianta je:

Použití:

---

## 3️⃣ Ještě praktičtější verze (procenta)

Tohle je už pěkná malá utilitka:

pak můžeš psát:

---

## 4️⃣ Kde to uložit

Do:

```markdown
~/.bashrc
```

nebo když chceš mít pěkné **Linuxové řešení**:

```markdown
~/bin/bright
```

a udělat z toho skript.

---

💡 Malý mentor-tip:  
V Linuxu je dobré si zapamatovat jednoduché pravidlo:

| věc | použít |
| --- | --- |
| krátká zkratka | alias |
| něco s parametry | funkce |
| něco většího | skript |

---

Jestli chceš, můžu ti ukázat ještě **velmi pěkný trik**:

jak si udělat **jednořádkovou funkci**, která vypadá skoro jako alias, ale **parametry umí**.  
Používají to hodně admini v `.bashrc`.

ano

Feďo, existuje velmi pěkný **admin trik**, který se používá v `.bashrc`, když chceš něco krátkého jako alias, ale zároveň chceš **parametry**.

Říká se tomu **one-liner bash funkce**.

---

## 1️⃣ Nejjednodušší forma

Použití:

Je to jen jedna řádka, ale pořád je to **plnohodnotná funkce**.

---

## 2️⃣ Varianta s kontrolou parametru

Tohle už je praktičtější:

Pak když zapomeneš parametr:

```markdown
bright
```

dostaneš:

---

## 3️⃣ Varianta s procenty (nejpohodlnější)

Tohle je moje oblíbená:

Použití:

---

## 4️⃣ Mini admin vychytávka

Můžeš si udělat **+10 / -10** jas:

Použití:

---

💡 Feďo, mimochodem – tohle je přesně styl práce, který jsi už používal u **PowerShell profile** (kde jsi měl funkce pro prompt a `.venv`).  
`.bashrc` v Linuxu je vlastně **stejná filozofie jako PowerShell profile.ps1**.

---

Jestli chceš, můžu ti ještě ukázat jednu **velmi cool věc**, kterou hodně používají admini:

udělat si **malou knihovnu funkcí** (např. `~/.bash_functions`) a jen ji načítat v `.bashrc`.  
Je to překvapivě užitečné, když těch funkcí začne být víc.

ano

Feďo, jakmile začneš mít víc funkcí, `.bashrc` se rychle změní v nečitelný chaos. Proto hodně adminů používá jednoduchý trik: **funkce mají v samostatném souboru**.

Je to vlastně malá **osobní knihovna shell nástrojů**.

---

## 1️⃣ Struktura

Uděláš soubor:

```markdown
~/.bash_functions
```

Například:

```markdown
nvim ~/.bash_functions
```

---

## 2️⃣ Do souboru dáš funkce

Například:

---

## 3️⃣ Načtení v.bashrc

Do `.bashrc` přidáš jen jeden řádek:

To znamená:

---

## 4️⃣ Aktivace

Stačí:

```markdown
source ~/.bashrc
```

nebo otevřít nový terminál.

---

## 5️⃣ Výsledek

Pak můžeš psát:

```markdown
bright 50
update
...
```

---

## 6️⃣ Proč to používají admini

Výhody:

- `.bashrc` zůstane krátký
- funkce jsou přehledně na jednom místě
- můžeš je **kopírovat mezi stroji**

Například:

```markdown
~/.bash_functions
~/.vimrc
~/.tmux.conf
```

a máš **stejné prostředí na každém Linuxu**.

---

## 7️⃣ Ještě lepší varianta (pro pokročilé)

Někteří lidé používají adresář:

a v `.bashrc`:

To je už **modulární bash konfigurace**.

---

💡 Feďo — tohle je mimochodem přesně styl práce, který by tě mohl bavit i pro tvůj **Arch výukový projekt**.

Můžeš si udělat třeba:

a budeš mít **vlastní mini toolbox pro Linux experimenty**.

---

Jestli chceš, můžu ti ještě ukázat jednu **extrémně užitečnou bash vychytávku**, kterou používá skoro každý Linux admin:

udělat si adresář

```markdown
~/bin
```

a mít tam vlastní příkazy jako normální programy (`bright`, `wifi`, `mountusb`, atd.).  
Je to ještě praktičtější než funkce.

ano

Feďo, tohle je jedna z nejlepších věcí, které si může linuxák udělat – **vlastní příkazy v `~/bin`**. Je to čisté, přenositelné a velmi unixové řešení.

Ukážu ti to krok za krokem.

---

## 1️⃣ Vytvoř adresář pro vlastní příkazy

---

## 2️⃣ Přidej ho do PATH

Podívej se, jestli už tam není:

```markdown
echo $PATH
```

Na většině systémů už tam bývá:

```markdown
/home/user/bin
```

Pokud tam není, přidej do `.bashrc`:

```markdown
export PATH="$HOME/bin:$PATH"
```

pak:

```markdown
source ~/.bashrc
```

---

## 3️⃣ Vytvoř první vlastní příkaz

Například **bright**.

```markdown
nvim ~/bin/bright
```

obsah:

---

## 4️⃣ Nastav spustitelnost

---

## 5️⃣ Použití

Teď můžeš psát odkudkoliv:

Stejně jako kdybys spustil systémový program.

---

## 6️⃣ Proč je to lepší než funkce

| řešení | výhody | nevýhody |
| --- | --- | --- |
| alias | rychlé | neumí parametry |
| funkce | parametry | jen v shellu |
| **~/bin skript** | funguje jako normální program | musíš vytvořit soubor |

Skript můžeš spustit:

- z bash
- z tmux
- z jiného shellu
- z cron
- z i3
- z jiného skriptu

---

## 7️⃣ Typické věci v ~/bin

Admini tam často mají:

například:

---

## 8️⃣ Malá unixová filozofie

Tohle je přesně princip:

```markdown
$HOME/bin
```

\= **osobní software**

Podobně jako:

```markdown
/usr/bin
/usr/local/bin
```

Ty máš prostě **svůj vlastní repo programů**.

---

## 9️⃣ Malý bonus (super praktický)

Když napíšeš:

```markdown
which bright
```

dostaneš:

```markdown
/home/feda/bin/bright
```

To znamená, že shell ho našel přes PATH stejně jako třeba `ls`.

---

💡 Feďo, myslím že **pro tvůj Arch laboratorní systém** je `~/bin` úplně ideální věc.  
Můžeš si tam postupně stavět vlastní nástroje jako:

```markdown
wifi-up
wifi-scan
net-info
boot-info
mount-labdisk
```

a budeš mít vlastní **malý admin toolkit**.

---

Jestli chceš, ukážu ti ještě jednu věc, která je mezi linuxáky skoro kultovní:

**jak si z `~/bin` udělat vlastní “balíčkovací systém” pro tvoje skripty** (velmi jednoduchý, ale překvapivě mocný).  
Používají to lidi, co mají hodně vlastních nástrojů.

Feďo, naopak — **`/usr/local/bin` je přesně místo, které je na to historicky určené**. 👍  
Jen je dobré pochopit rozdíl mezi třemi typickými místy.

---

## 1️⃣ /usr/bin

Programy instalované **balíčkovacím systémem**.

Např.:

```markdown
/usr/bin/ls
/usr/bin/git
/usr/bin/python
```

Ty spravuje:

```markdown
pacman
apt
dnf
```

Na ty **by se ručně sahat nemělo**.

---

## 2️⃣ /usr/local/bin ✅

Programy instalované **lokálním administrátorem**.

Historicky to znamenalo:

> software instalovaný mimo balíčkovací systém distribuce

Typicky:

```markdown
/usr/local/bin
/usr/local/lib
/usr/local/share
```

Sem patří:

- vlastní skripty
- ručně kompilované programy
- nástroje mimo pacman/apt

Takže například:

a pak:

```markdown
bright 50
```

---

## 3️⃣ ~/bin

Programy **jen pro konkrétního uživatele**.

Výhody:

- nepotřebuješ sudo
- nepřepisuješ systém
- snadno přeneseš domovský adresář

---

## 4️⃣ Praktické pravidlo adminů

| místo | kdy použít |
| --- | --- |
| `~/bin` | osobní skripty |
| `/usr/local/bin` | skripty pro celý systém |
| `/usr/bin` | pouze balíčky |

---

## 5️⃣ Příklad z reálného serveru

Typický obsah:

To jsou **admin skripty**.

---

## 6️⃣ Další dobrá praxe

Často se používá struktura:

```markdown
/usr/local/bin
/usr/local/lib
/usr/local/share
/usr/local/src
```

např.:

```markdown
/usr/local/bin/mytool
/usr/local/share/mytool/config.yaml
```

---

## 7️⃣ Drobná výhoda /usr/local/bin

Je v PATH **dříve než `/usr/bin`** na mnoha systémech.

Takže můžeš **přepsat chování nástroje**.

Například:

```markdown
/usr/local/bin/python
/usr/bin/python
```

Shell použije první.

---

💡 Feďo, pro tvůj **Arch laboratorní projekt** bych doporučil jednoduché pravidlo:

To je velmi přirozený vývoj.

---

Jestli chceš, můžu ti ještě ukázat **jednu hodně geekovskou věc**, kterou dělají admini:

jak mít vlastní nástroje v `/usr/local/bin`, ale jejich zdrojáky mít v git repozitáři třeba:

```markdown
~/lab-tools
```

a instalovat je jedním příkazem.  
Je to vlastně **mini balíčkovací systém pro vlastní skripty**.

Feďo, tohle je velmi dobrá otázka, protože na tom je vidět **jak funguje pipe, sudo a přesměrování v shellu**. Spousta lidí to používá, ale nerozumí proč.

Podíváme se na to krok za krokem.

---

## 1️⃣ Co dělá tee

`tee` je program, který:

- čte **stdin**
- zapíše ho **do souboru**
- zároveň ho vypíše na **stdout**

Příklad:

výsledek:

```markdown
Ahoj
```

a zároveň vznikne soubor:

```markdown
soubor.txt
Ahoj
```

---

## 2️⃣ Co znamená pipe |

Pipe znamená:

tedy:

ASCII tok dat:

---

## 3️⃣ Proč tam je sudo

Soubor

```markdown
/sys/class/backlight/intel_backlight/brightness
```

patří rootovi.

Když zkusíš:

dostaneš:

```markdown
permission denied
```

---

## 4️⃣ Proč nefunguje sudo echo

Tohle je klasická past:

NEFUNGUJE.

Proč?

Protože **přesměrování `>` provádí shell, ne echo**.

Takže shell se snaží otevřít soubor **bez sudo**.

ASCII:

---

## 5️⃣ Proč funguje sudo tee

Tady se rootem stane program **tee**.

ASCII:

`tee` běží jako root → může zapisovat.

---

## 6️⃣ Ano — bude to chtít heslo

Poprvé:

Pak si sudo chvíli pamatuje autentizaci (typicky **5 minut**).

---

## 7️⃣ Alternativy

## varianta 1 (nejčistší)

---

## varianta 2

Tady běží **celý shell jako root**.

---

## varianta 3 (nejhorší)

zbytečně otevírá root shell.

---

## 8️⃣ Malý detail

Protože `tee` vypisuje stdout, zobrazí se číslo:

```markdown
500
```

Když to nechceš vidět:

---

## 9️⃣ Malá linuxová filozofie

Toto je krásný příklad unixového principu:

```markdown
echo
tee
sudo
pipe
```

Každý nástroj dělá **jednu jednoduchou věc**.

---

Feďo, jestli chceš, můžu ti ještě ukázat **jednu zajímavou věc na `/sys/class/backlight`**, která většině lidí nedojde:

proč někdy **max\_brightness není skutečný maximální jas displeje** a proč někdy **funguje jen intel\_backlight a acpi\_video0 ne**.  
To je docela zajímavá věc z oblasti kernelu a ACPI.