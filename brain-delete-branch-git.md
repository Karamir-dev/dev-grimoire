# 🧹 Nettoyer les branches Git supprimées du serveur distant

## 🔄 1. Supprimer les références aux branches distantes supprimées

```bash
git fetch -p
```
✅ Cette commande supprime les références aux branches supprimées sur le serveur (par exemple origin/feature-xyz) mais ne touche pas aux branches locales.

---

## 🧨 2. Supprimer les branches locales qui ne sont plus suivies à distance

```bash
git remote prune origin && git branch -vv | awk '/: gone]/{print $1}' | xargs -r git branch -d
```

📌 Explication :
- `git remote prune origin` : nettoie les références distantes obsolètes.
- `git branch -vv` : liste toutes les branches locales avec leur tracking.
- `awk '/: gone]/{print $1}'` : sélectionne les branches locales dont la branche distante a été supprimée.
- `xargs -r git branch -d` : supprime ces branches localement (si elles ont été mergées).

## 💣 Pour forcer la suppression des branches locales (non mergées)
```bash
git branch -vv | awk '/: gone]/{print $1}' | xargs -r git branch -D
```
⚠️ Utiliser avec précaution : cela supprime des branches locales même si elles n’ont pas été mergées.
---
## 💡 Astuce : créer un alias Git

Ajoute ceci à ton fichier ~/.gitconfig :
```ini
[alias]
  clean-dead-branches = "!git remote prune origin && git branch -vv | awk '/: gone]/{print $1}' | xargs -r git branch -d"
```

Puis utilise simplement :
```bash
git clean-dead-branches
```
