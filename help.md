# Aide de mon `.zshrc`

Ce document résume les commandes/fonctions présentes dans mon `.zshrc`.

## 1) Actions automatiques au démarrage du shell

| Élément | Rôle |
|---|---|
| `custom_prompt` + `precmd` | Construit un prompt personnalisé (`user@host:path$`) avec couleurs, raccourci `~`, support chemin SFTP et alias visuel `mega`. |
| `setxkbmap ... \| xkbcomp ...` | Recharge immédiatement le mapping clavier personnalisé (`custom`, variante `ctrl_accents`). |
| `if [[ $PWD == /run/user/.../sftp:host=pc.teamgeek.fr,port=4339/* ]]; then ssh ...; fi` | Si le terminal est lancé dans ce montage SFTP, ouvre automatiquement une session SSH vers `pc.teamgeek.fr` (port 4339). |
| `if [[ "$TERM_PROGRAM" != "vscode" ]]; then echo ...; fi` | Affiche un banner ASCII vert au lancement, sauf dans le terminal intégré VS Code. |
| Rotation log bisync | Si `bisync.log` existe, conserve seulement les 5000 dernières lignes. |
| Auto-start bisync | Lance `unison bisync -silent` au démarrage si aucun process équivalent n’est déjà actif. |
| `compdef _bisync_completion bisync` | Active l’autocomplétion Zsh pour la commande `bisync`. |
| `export PATH="$HOME/.local/bin:$PATH"` | Ajoute `~/.local/bin` au `PATH`. |

## 2) Alias

| Alias | Équivalent | Description |
|---|---|---|
| `reload_keyboard` | `setxkbmap ... \| xkbcomp ...` | Recharge à la demande le layout clavier custom. |
| `gcl` | `git clone` | Raccourci de clonage Git. |
| `sgoinfre` | `cd /sgoinfre/goinfre/Perso/tlair` | Va directement dans ton dossier perso goinfre. |
| `v` | `valgrind` | Lance Valgrind rapidement. |
| `vfull` | `valgrind --leak-check=full --show-leak-kinds=all --track-fds=yes --trace-children=yes --track-origins=yes` | Profil Valgrind complet pour debug mémoire/FD/process enfants. |
| `clear` | `/usr/bin/clear && source ~/.zshrc` | Nettoie l’écran puis recharge le `.zshrc`. |
| `makeminishell` | `make re > /dev/null && make clean > /dev/null && ./minishell` | Recompile minishell silencieusement puis l’exécute. |
| `cc` | `cc -Wall -Wextra -Werror` | Compilation C stricte par défaut. |
| `gcc` | `gcc -Wall -Wextra -Werror` | Même principe mais via `gcc`. |

## 3) Fonctions utilisateur (commandes principales)

### `help`
- Ouvre la page d’aide distante GitHub dans le navigateur (`xdg-open`).
- Usage: `help`

### `bisync`
- Pilote `unison` pour synchroniser avec le cloud `mega`.
- Sous-commandes: `start`, `stop`, `restart`, `status`, `logs`, `help`.
- Usage: `bisync start`

### `maker`
- Build `make re` en silencieux, récupère `TARGET=` dans `Makefile`, exécute le binaire, puis `make fclean`.
- Option `-v` pour lancer via Valgrind.
- Usage: `maker -v arg1 arg2`

### `curlexec`
- Télécharge un script via `curl` et l’exécute avec `bash`.
- Usage: `curlexec https://example.com/script.sh`

### `test`
- Si dossier contient `Minishell`: lance `tester.sh`.
- Sinon, lance `test.sh` s’il existe.
- Sinon, compile les `.c` (ou arguments fournis) avec `cc`, exécute `a.out`, puis le supprime.
- Usage: `test` ou `test main.c utils.c`

### `ssh` (wrapper)
- Redirige des alias courts:
- `ssh teamgeek.fr` -> `ssh tlair@185.249.72.46`
- `ssh pc.teamgeek.fr` -> `ssh -p 4339 tlair@pc.teamgeek.fr`
- Sinon, comportement SSH normal.
- Usage: `ssh teamgeek.fr`

### `bigfiles`
- Affiche le top 30 des plus gros fichiers modifiables de `~` (taille en MB).
- Option `-supp` pour sélectionner par index puis proposer suppression.
- Usage: `bigfiles` / `bigfiles -supp 1 2`

### `bigfolders`
- Affiche le top 50 des plus gros dossiers sous `/home/tlair` (profondeur max 4).
- Option `-supp` pour sélectionner par index puis proposer suppression récursive.
- Garde-fou: interdit explicitement `/`, `/home`, `/home/tlair`.
- Usage: `bigfolders` / `bigfolders -supp 1 3`

### `spaceleft`
- Affiche la taille de `/home/tlair` en MiB, le % d’occupation, et l’espace restant par rapport à une cible (5000 MiB par défaut).
- Usage: `spaceleft` / `spaceleft 6000`

### `gpush`
- Chaîne `git add .` -> `git commit -m "<message>"` -> `git push` avec messages d’erreur colorés en cas d’échec.
- Usage: `gpush "feat: update parser"`

### `setup_ssh_keys`
- Crée `~/.ssh`, écrit une paire de clés ED25519 (privée/publique), fixe les permissions, lance `ssh-agent`, puis ajoute la clé.
- Usage: `setup_ssh_keys`

### `tobin`
- Convertit des nombres décimaux en binaire via `bc`.
- Usage: `tobin 10 42 255`

### `norm`
- Surcouche `norminette` avec options:
- `-exclude` pour ignorer fichiers/dossiers
- `-ok` pour cacher les lignes `OK!`
- Si aucun fichier fourni, scan récursif des `*.c`/`*.h` (hors certains fichiers de test).
- Affiche un résumé final (`Everything OK` / `Norm error`) + détection des globales.
- Usage: `norm -exclude build test.c -ok`

### `ginit`
- Initialise un repo Git local, configure `origin`, passe en `main`, puis pousse `main` en upstream.
- Usage: `ginit git@github.com:user/repo.git`

### `greinit`
- Réinitialise complètement Git dans le dossier courant (`rm -rf .git` si présent), recrée le dépôt, commit initial, configure `origin`, fetch/merge `origin/main`.
- Usage: `greinit git@github.com:user/repo.git`

### `checkleaks`
- Génère un fichier de suppressions temporaire pour Valgrind (notamment readline), lance Valgrind avec options complètes, puis supprime le fichier temporaire.
- Usage: `checkleaks ./minishell`