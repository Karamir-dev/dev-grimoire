# 🧙‍♂️ Mon terminal, mon royaume : alias, prompt, et magie noire
Bienvenue dans les profondeurs d’un royaume où chaque commande est un sort, chaque raccourci une rune, et où le prompt est le trône de ton pouvoir : ton terminal.

Dans cet article, on va faire de ton terminal un véritable grimoire interactif avec :
- Des alias pratiques (Zsh/Bash)
- Un prompt stylé avec Starship
- Quelques incantations secrètes d’Oh My Zsh
---

## ⚔️ 1. Les alias : des sorts rapides pour paresseux efficaces

Un **alias**, c’est comme un raccourci clavier... mais pour le shell. Voici quelques classiques :

```bash
alias gs='git status'
alias ga='git add .'
alias gc='git commit -m'
alias gpo='git push origin'
alias ..='cd ..'
alias ...='cd ../..'
alias ll='ls -alF'
alias cls='clear'
```
> 🔮 Pro-tip : ajoute-les dans ton ~/.zshrc ou ~/.bashrc pour les charger à chaque session.
---

## 2. Ton prompt, ton miroir : l’élégance avec Starship

### 🌟 Installation rapide
```bash
# Linux (avec bash ou zsh)
curl -sS https://starship.rs/install.sh | sh
```
Puis ajoute dans ton fichier de config :
```bash
# Pour Bash
echo 'eval "$(starship init bash)"' >> ~/.bashrc

# Pour Zsh
echo 'eval "$(starship init zsh)"' >> ~/.zshrc
```
### ✨ Pourquoi c’est magique ?
Starship affiche dynamique ton environnement :

- le dossier courant
- la branche git
- le statut du repo
- le langage détecté (Python, Node.js…)
- le temps d’exécution de la dernière commande
- etc.

### 🧪 Exemple de configuration (dans `~/.config/starship.toml`)
```toml
[directory]
truncation_length = 3
truncate_to_repo = false

[git_status]
format = '([$all_status$ahead_behind] )'

[python]
symbol = '🐍 '
```
---
## 🧙 3. Oh My Zsh : la suite magique du sorcier moderne
Oh My Zsh est un **framework de configuration Zsh** qui ajoute plugins, thèmes et raccourcis.

### Installation
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
> 🧪 Il modifie ton `.zshrc` automatiquement.

### 🔌 **Plugins utiles à activer dans ton** `.zshrc`
```bash
plugins=(git z sudo extract zsh-autosuggestions zsh-syntax-highlighting)
```
Installe les plugins additionnels avec :
```bash
# Autosuggestions
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# Syntax highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
---
## 💡 Bonus : quelques sorts secrets à connaître
### Exécuter la dernière commande en root
```bash
sudo !!
```
### **Rechercher dans ton historique avec** `Ctrl + R`
Tape un mot-clé, retrouve ta commande (recherche dans ton historique), magie.
### **Coloriser** `man` :
Ajoute dans ton `.bashrc` ou `.zshrc` :
```bash
export LESS_TERMCAP_md=$'\e[01;31m'
export LESS_TERMCAP_me=$'\e[0m'
export LESS_TERMCAP_us=$'\e[01;32m'
export LESS_TERMCAP_ue=$'\e[0m'
```
---

## 📜 **Conclusion**
Ton terminal peut être bien plus qu’un simple outil : **un bastion de productivité**, une extension de toi-même.

À toi maintenant d’y graver tes alias, d’y forger ton prompt, et de régner sans latence sur ton royaume.
