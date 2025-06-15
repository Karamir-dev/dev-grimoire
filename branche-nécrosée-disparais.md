# 🧙 Sortilège Git Avancé – *Branche Nécrosée, Disparais !*

> *"Par le feu sacré de Git, que les branches mortes rejoignent le néant."*  
> – Extrait du Grimoire de la Propreté Dépôtale

---

## ✨ Effet du sort

Ce sortilège supprime **automatiquement** :

- 🪓 les **branches distantes** disparues sur le remote (`origin`)  
- 💀 les **branches locales** orphelines (celles qui suivaient une branche distante supprimée)  
- 🔒 tout en protégeant la **branche actuelle** (car on n’est pas des kamikazes du commit)

---

## 📜 Formule magique (Script)

Copie ce sort dans un script bash, appelle-le `git-nuke-branches.sh`, et rends-le exécutable :

```bash
#!/bin/bash

echo "🧙‍♂️ Invocation du sortilège de purge..."

# Étape 1 – Appel aux forces du prune
git remote prune origin

# Étape 2 – Repérage de la branche du sorcier actuel
current_branch=$(git symbolic-ref --short HEAD)

# Étape 3 – Inspection de toutes les branches locales
for branch in $(git for-each-ref --format='%(refname:short)' refs/heads/); do
    if [[ "$branch" == "$current_branch" ]]; then
        continue  # Ne pas se suicider
    fi

    # Scrutation du lien entre la branche locale et son ancêtre distant
    upstream=$(git for-each-ref --format='%(upstream:short)' "refs/heads/$branch")
    if [[ -n "$upstream" ]]; then
        if ! git show-ref --verify --quiet "refs/remotes/$upstream"; then
            echo "💨 Incinération de la branche locale orpheline : $branch (anciennement $upstream)"
            git branch -D "$branch" &>/dev/null
        fi
    fi
done

echo "✅ Sort terminé. Le dépôt est purifié."
```

---

## 🪄 Alias dans ton `.gitconfig`

Tu veux lancer ça d’un coup de baguette magique ?

Ajoute à ton `.gitconfig` :

```ini
[alias]
    nuke = "!bash ~/.git-nuke-branches.sh"
```

Et hop :

```bash
git nuke
```

---

## 📦 Option bonus : l’automatiser ?

Tu peux attacher ce sort à un **hook `post-pull`** si tu veux que la purge se déclenche à chaque mise à jour du dépôt :

```bash
echo -e "#!/bin/bash\nbash ~/.git-nuke-branches.sh" > .git/hooks/post-pull
chmod +x .git/hooks/post-pull
```

---

## 🧠 Notes de l'Archimage Git

- Ce sort respecte la branche active et ne la touchera jamais.
- Il ne supprime que les branches locales **liées à un remote supprimé**.
- Les branches locales non suivies (`--no-upstream`) ne sont pas concernées.

---

## 📖 Extrait du Grimoire

> *"Toute branche qui ne vit plus sur le serveur, doit être effacée sans remords.  
> Le dépôt ne tolère ni les fantômes, ni les oubliés."*

---

## 🧤 À retenir

- Utilise ce sort avant une réunion d’équipe pour briller comme un Mage Git organisé.
- En cas d’accident (tu es humain, pas demi-elfe), pense à `git reflog` pour la résurrection.

---

🎩 Et voilà, tu viens d’ajouter à ton Grimoire un des sortilèges Git les plus pratiques.  
Un vrai gain de temps. Un nettoyage parfait. Un dépôt sain.

> 🧙 *Code propre, esprit clair.*
