# ⚰️ Git : Nettoyer les branches mortes comme un fossoyeur de repo

> Tu clones, tu crées, tu merges… mais tu oublies de faire le ménage ?
> 
> Il est temps de sortir la pelle, car ton dépôt local est devenu un cimetière de branches.
> 
> Voici comment jouer les fossoyeurs et enterrer proprement les branches locales supprimées sur le dépôt distant.

## 🧹 1. Le problème : des branches locales orphelines

Quand tu supprimes une branche sur un remote (`origin` par exemple), Git **ne supprime pas automatiquement la branche locale correspondante**.

Tu te retrouves donc avec des branches zombies : inutiles, mais toujours là.

---

## 🔍 2. Repérer les branches mortes

Tu peux repérer les branches locales qui n’ont plus de correspondance à distance avec :

```bash
git fetch -p
git branch -vv
```
Les branches marquées [gone] sont celles dont le remote n’existe plus :
```bash
  feature/old-stuff   abc1234 [gone]        Une vieille branche qui pue
```
---
## 💀 3. Les enterrer automatiquement
Voici la commande magique pour supprimer toutes les branches locales dont le remote a disparu :
```bash
git fetch -p
git branch -vv | grep ': gone]' | awk '{print $1}' | xargs -r git branch -d
```
> 🧙 Explication du sort :
>
> - `git fetch -p` : récupère les updates du remote et "prune" les références mortes.
> - `git branch -vv` : liste les branches avec leur remote.
> - `grep ': gone]'` : filtre celles qui n'ont plus de remote.
> - `awk '{print $1}'` : extrait le nom de la branche.
> - `xargs git branch -d` : supprime les branches localement.
---
## ☠️ 4. Et si la branche n’est pas mergée ? Utilise le `-D`

La commande précédente utilise `-d` (safe delete) qui échoue si la branche n’est pas mergée.

Si tu veux forcer leur suppression (attention danger ⚠️) :
```bash
git branch -vv | grep ': gone]' | awk '{print $1}' | xargs -r git branch -D
```
---
## 🧼 5. Astuce bonus : alias dans ton `.bashrc` ou `.zshrc`
Ajoute ceci pour invoquer ton nettoyage avec un simple `git-clean-branches` :
```bash
alias git-clean-branches='git fetch -p && git branch -vv | grep ": gone]" | awk "{print \$1}" | xargs -r git branch -d'
```
---
## 📜 Conclusion

Ton repo est vivant. Il mérite un environnement propre et sain.

Fais le ménage de temps en temps : **ni tes collègues ni ton futur toi ne te remercieront, mais ils devraient**.
