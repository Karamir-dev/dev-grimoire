# 🧙 Script du jour : `git-wtf.sh` – Le sortilège de clarté Git

> _"Sur quelle branche suis-je ? Est-elle à jour ? Ai-je des commits non poussés ?"_ \
> Stop à l’amnésie Git. Ce script t’offre un état des lieux rapide de ton repo. En une commande, tu sais si tu es safe… ou en danger.

## 🧾 Le besoin

Quand tu bosses sur plusieurs projets, tu perds vite la vue d’ensemble. Avant de push, de merge ou de pull : tu dois savoir si…

- tu es sur une branche à jour
- tu as des commits non poussés
- tu es en avance / en retard
- tu as des fichiers modifiés ou non suivis

## 🔮 La solution : un script magique `git-wtf.sh`

```bash
#!/bin/bash

# ┌──────────────────────────────┐
# │ Git WTF - état du dépôt Git │
# └──────────────────────────────┘

# Détecte le chemin du repo (par défaut: dossier courant)
TARGET_PATH="${1:-$(pwd)}"

# Vérifie si c'est un dépôt git
if [ ! -d "$TARGET_PATH/.git" ] && [ ! -d "$TARGET_PATH/.git/modules" ]; then
  echo "❌ Aucun dépôt Git trouvé dans : $TARGET_PATH"
  exit 1
fi

# Va dans le répertoire (sans affecter le shell parent)
cd "$TARGET_PATH" || exit

echo "📦 Repo : $(basename "$(git rev-parse --show-toplevel)")"
echo "📁 Chemin : $TARGET_PATH"
echo "🌿 Branche : $(git rev-parse --abbrev-ref HEAD)"
echo

# Statut par rapport au remote
echo "🔁 Statut de synchronisation :"
git status -sb

# Statut des fichiers
echo
echo "📝 Modifications locales :"
git status --short

# Derniers commits
echo
echo "📜 Derniers commits :"
git log --oneline -n 5

echo
echo "🧙 Conseil : ‘git pull --rebase && git push’ si tout va bien !"
```
## 🧪 Utilisation magique :

```bash
# 1. Sur le dossier courant
git-wtf.sh

# 2. Sur un autre projet
git-wtf.sh ~/projets/distorsion

# 3. En alias plus classe
alias git-wtf='~/scripts/git-wtf.sh'
```

## ⚙️ Installation (dans ton `$PATH`)

1. Sauvegarde ce script dans un fichier nommé `git-wtf.sh`
2. Rends le exécutable
```bash
chmod +x ~/scripts/git-wtf.sh
```
3. (Optionnel enlève virtuellement le `.sh`)
```bash
ln -s ~/scripts/git-wtf.sh /opt/scripts/git-wth
```
4. Ajoute-le à ton PATH (ex : dans `.bashrc` ou `.zshrc`) :
```bash
export PATH="/opt/scripts:$PATH"
```
> Note : que cette commande ajoutera tout tes scripts comme des commandes.
Tu peux maintenant l’utiliser avec :
```bash
git-wtf
```

Si modifié ton `$PATH` te fait peur tu peux :
```bash
alias git-wtf="$HOME/scripts/git-wtf.sh"
```

## 🧪 Exemple du résultat du sortilège
```text
📦 Repo: mon-super-projet
🌿 Branche actuelle: feature/nouvelle-fonction

🔁 Statut de synchronisation :
## feature/nouvelle-fonction...origin/feature/nouvelle-fonction [ahead 2]

📝 Fichiers modifiés :
 M README.md
?? new-file.txt

📜 Derniers commits :
d47a1b2 Ajout du fichier de config
e6f2c3d Correction du bug de login
...
```

## 🎁 Bonus : extension possible
Tu peux enrichir le script pour :
- détecter si tu es dans un merge ou un rebase
- afficher les branches locales/à supprimer
- coloriser les messages
- faire un résumé multi-repos avec `find . -name .git`

## 🧰 Astuce bonus : détection avancée (même si le .git est plus haut)
Si tu veux rendre la détection encore plus magique (cas des sous-dossiers de repo), remplace le test .git par :
```bash
git -C "$TARGET_PATH" rev-parse --is-inside-work-tree >/dev/null 2>&1 || {
  echo "❌ Aucun dépôt Git trouvé dans : $TARGET_PATH"
  exit 1
}
```
> Et utilise `git -C "$TARGET_PATH" <commande>` à la place de cd.

## 📜 Conclusion
Ce petit script devient vite indispensable pour :
- éviter les erreurs de push
- faire ton "check up" matinal de dev
- bosser plus vite avec Git sans oublier un add traître
