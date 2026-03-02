### 1. Instalace na Debianu

Protože jsi na Debianu, instalace je přímočará (vyžaduje práva sudo):
```
sudo apt update
sudo apt install git -y
```

### 2. Život v terminálu: Workflow bez myši

Ve VS Code jsi zvyklý na tlačítka "Stage" a "Commit". V terminálu to vypadá takto:

Založení repozitáře: Přejdi do složky s projektem a zadej
`git init`

Stav souborů: Místo barevných ikon v postranním panelu používej neustále:
`git status`

Přidání a commit:
```sh
git add .
git commit -m "Tvoje zpráva"
```

### 3. Práce se vzdáleným repozitářem GitHub

Jelikož nemáš prohlížeč pro klikání na "Sign in", budeš muset použít buď SSH klíče (které jsem zmiňoval minule), nebo Personal Access Token (PAT).
Jak na SSH v terminálu:

Pokud už máš klíč vygenerovaný (ssh-keygen), jeho obsah si vypíšeš takto:

`cat ~/.ssh/id_ed25519.pub`

Ten pak vložíš do nastavení GitHubu přes jiný počítač, kde prohlížeč máš. Poté si projekt naklonuješ pomocí SSH adresy:

`git clone git@github.com:uzivatel/projekt.git`

### 4. Prohlížení historie

Ve VS Code máš grafické rozšíření (GitLens), v terminálu je to bez úprav trochu nepřehledné. Zkus tento "magický" příkaz pro hezký výpis historie:

`git log --oneline --graph --all`

Můžeš si na to vytvořit alias, aby ses nemusel tolik upisovat:

`git config --global alias.lg "log --oneline --graph --all"`

Pak už ti bude stačit psát jen `git lg`

### 5. Souhrn pro Debian CLI

Instalace: `apt install git`

Identita: Nezapomeň na `git config --global user.name a email`, jinak tě Git nepustí ke commitu.

Bezpečí: `SSH` je na serveru tvůj nejlepší přítel (mnohem lepší než zadávat heslo u každého pushování).

Orientace: `git status` je tvůj kompas, `git log` tvoje historie.