# 🪄 Rituel — L’Invocateur de Coquille Partagée

> *"Quand deux âmes s’unissent sous le même terminal, le code devient incantation, et le bug un adversaire à deux lames."*

---
## 📚 Savoir ancien
Ce rituel te permet de **partager un terminal** avec un·e acolyte du code.  
Plus besoin de streamer ton écran, ni de copier tes sorts sur Discord.  
Grâce à `tmux`, `tmate`, ou l’antique `screen`, vous écrirez ensemble sur la **même coquille sacrée**.

---
## 🧰 Ingrédients
- 🧙‍♀️ Un Maître : celui qui ouvre la faille du terminal
- 🧑‍💻 Un Acolyte : celui qui s’y connecte
- 🔐 `tmux` (ou `tmate`) pour ouvrir le portail partagé
- 🌐 Un accès SSH ou un tunnel éphémère
- (Optionnel) Un même utilisateur, ou une socket partagée via `/tmp`

---
## ⚔️ Variante 1 — Le Lien du Sang (`tmux` + `ssh`)
### 🧙‍♂️ L’Hôte prépare l’autel :
```bash
tmux new-session -s sanctuaire
```
### 🧑‍🚀 L’Acolyte franchit le portail :
```bash
ssh ton_user@adresse_ip
tmux attach-session -t sanctuaire
```
💡 Deux utilisateurs différents ? Crée une socket magique :
```bash
tmux -S /tmp/partage.sock new -s sanctuaire
chmod 777 /tmp/partage.sock
```
Et l’Acolyte :
```
tmux -S /tmp/partage.sock attach -t sanctuaire
```
---

### ✨ Sortilèges utiles de `tmux`

| Sortilège                              | Effet                                             |
|----------------------------------------|---------------------------------------------------|
| `Ctrl+b %`                             | Clive l’écran en deux colonnes                    |
| `Ctrl+b "`                             | Scinde l’écran en deux lignes                     |
| `Ctrl+b o`                             | Change de fenêtre                                 |
| `Ctrl+b [`                             | Voyage dans l’historique des runes passées       |
| `Ctrl+b :setw synchronize-panes on`    | Synchronise les incantations dans tous les panneaux |
| `Ctrl+b d`                             | Détache la transe sans fermer le portail          |

---

## 🌩️ Variante 2 — Le Tunnel du Néant (`tmate`)

> *Pour les magicien·nes pressé·es ou éloigné·es dans un autre royaume réseau.*

### 🪄 L’Hôte incante :

```
sudo apt install tmate
tmate
```

Tu recevras alors :
```
ssh abcdef@ny.tmate.io
```
🧑‍🚀 Ton acolyte n’a plus qu’à se téléporter via le lien.  
💡 Utilise le read-only pour les démonstrations ou formations.

---

## 🧙‍♂️ Variante Bonus — Le Grimoire Ancien : `screen`

> *Quand `tmux` est indisponible ou capricieux, `screen` prend le relais avec la sagesse des vieux terminaux.*

### 🧾 Invocation :

**L’Hôte :**
```
screen -S sanctuaire
```
**L’Acolyte :**
```
ssh ton_user@ip
screen -x sanctuaire
```
---

### 📜 Sortilèges anciens de `screen`

| Sortilège         | Effet                                             |
|-------------------|---------------------------------------------------|
| `Ctrl+a d`        | Détache la session                                |
| `screen -ls`      | Liste les sessions existantes                     |
| `screen -r nom`   | Rejoint une session détachée                      |
| `Ctrl+a c`        | Crée une nouvelle fenêtre                         |
| `Ctrl+a n / p`    | Navigue entre les fenêtres                        |
| `Ctrl+a k`        | Tue la fenêtre active                             |
| `Ctrl+a ?`        | Affiche l’aide des raccourcis (car l’oubli guette) |

---

## 🛡️ Précautions de mage

- 🚪 Ferme toujours tes portails à la fin (`exit`, `tmux kill-session`, etc.)
- 🔐 Ne laisse jamais traîner un accès SSH actif après le rituel
- ☁️ `tmate` utilise des serveurs distants → ne pas l’utiliser pour des données sensibles
- 📦 `screen` peut être une bonne alternative sur des serveurs minimalistes

---

## 🧠 Leçon de sagesse

> Deux claviers, un écran.  
> Un terminal, deux esprits.  
> Le code n’est plus individuel : il devient **partage, entraide et sorcellerie productive**.
