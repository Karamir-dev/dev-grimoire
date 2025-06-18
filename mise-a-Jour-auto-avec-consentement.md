# 🧙 Rituel d'Éveil du Système : Mise à Jour avec Consentement

> *"Il n’est de plus grande folie que de croire son système immortel."* – Sage Sysadmin

---

## 🧭 Objectif du rituel

Dans ce chapitre du grimoire, nous allons accomplir un **rituel sacré** qui :

1. Se lance automatiquement chaque jour via un **cron job**.
2. **Ouvre un terminal graphique** pour la cérémonie.
3. **Demande ton autorisation** avant d'agir.
4. Laisse le terminal **ouvert pour que tu observes les augures.**

Ce rite est idéal pour les mages du quotidien qui veulent garder le contrôle sur leurs mises à jour, sans y penser chaque matin.

---

## 🧪 Ingrédients nécessaires

- Un système GNU/Linux avec interface graphique
- Un terminal installé (`gnome-terminal`, `konsole`, `xfce4-terminal`, ou `xterm`)
- Un accès `sudo`
- `cron` activé pour l'utilisateur courant
- Un brin de prudence

---

## 📜 Rituel d’Invocation du Terminal (`interactive-update-launcher.sh`)

Ce script détecte le terminal disponible et y lance le **rituel principal**.

```bash
#!/bin/bash

# === Lance le script de mise à jour dans un terminal ===

SCRIPT_PATH="$HOME/.local/bin/daily-update.sh"

if command -v gnome-terminal >/dev/null; then
    gnome-terminal -- bash -c "$SCRIPT_PATH; echo ''; echo 'Appuyez sur Entrée pour fermer...'; read"
elif command -v konsole >/dev/null; then
    konsole --noclose -e bash -c "$SCRIPT_PATH"
elif command -v xfce4-terminal >/dev/null; then
    xfce4-terminal --hold -e "bash -c '$SCRIPT_PATH'"
elif command -v xterm >/dev/null; then
    xterm -hold -e "bash -c '$SCRIPT_PATH'"
else
    echo "Aucun terminal graphique trouvé pour lancer le script."
    exit 1
fi
```

📌 **À placer dans** : `~/.local/bin/interactive-update-launcher.sh`  
💾 **N'oublie pas** : `chmod +x ~/.local/bin/interactive-update-launcher.sh`

---

## 📜 Rituel Principal (`daily-update.sh`)

Ce script demande à l’utilisateur s’il accepte de lancer la mise à jour. En cas de réponse positive, il exécute la commande adaptée à ta distribution.

```bash
#!/bin/bash

DISTRO=$(grep "^ID=" /etc/os-release | cut -d= -f2 | tr -d '"')
DATE=$(date '+%Y-%m-%d %H:%M:%S')
LOGFILE="$HOME/.local/logs/update.log"

mkdir -p "$(dirname "$LOGFILE")"

case "$DISTRO" in
    ubuntu|debian)
        UPDATE_CMD="sudo apt update && sudo apt upgrade -y"
        ;;
    arch|manjaro)
        UPDATE_CMD="sudo pacman -Syu"
        ;;
    fedora)
        UPDATE_CMD="sudo dnf upgrade -y"
        ;;
    opensuse*)
        UPDATE_CMD="sudo zypper update -y"
        ;;
    *)
        echo "$DATE [ERROR] Distribution non reconnue : $DISTRO" >> "$LOGFILE"
        exit 1
        ;;
esac

echo -e "\n[$DATE] Souhaitez-vous lancer la mise à jour système maintenant ? [o/N]"
read -r response

if [[ "$response" =~ ^[Oo]$ ]]; then
    echo "[$DATE] Lancement de la mise à jour…" | tee -a "$LOGFILE"
    echo "Commande utilisée : $UPDATE_CMD" | tee -a "$LOGFILE"
    bash -c "$UPDATE_CMD" 2>&1 | tee -a "$LOGFILE"
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] ✅ Mise à jour terminée." | tee -a "$LOGFILE"
else
    echo "[$DATE] ❌ Mise à jour annulée par l'utilisateur." | tee -a "$LOGFILE"
fi
```

📌 **À placer dans** : `~/.local/bin/daily-update.sh`  
💾 **Et aussi** : `chmod +x ~/.local/bin/daily-update.sh`

---

## ⏰ Rituel de Programmation dans le Temps (CRON)

Ajoute ce rituel à ton `crontab` utilisateur avec :

```bash
crontab -e
```

Et insère :

```cron
0 9 * * * DISPLAY=:0 XAUTHORITY=$HOME/.Xauthority ~/.local/bin/interactive-update-launcher.sh
```

### 📌 Déchiffrage des runes :

| Élément       | Signification                                  |
|---------------|------------------------------------------------|
| `0 9 * * *`   | Chaque jour à 9h00                             |
| `DISPLAY=:0`  | Cible l'affichage graphique actif              |
| `XAUTHORITY`  | Permet à cron de parler au serveur X           |
| `launcher.sh` | Lance le rituel dans un terminal               |

---

## 🧪 Vérification du rituel

Tu peux tester manuellement avec cette commande magique :

```bash
DISPLAY=:0 XAUTHORITY=$HOME/.Xauthority ~/.local/bin/interactive-update-launcher.sh
```

---

## 🧠 Astuce de Mage Supérieur

- 🔔 Tu peux ajouter une **notification sonore ou graphique** avant d'ouvrir le terminal.
- 🛑 Tu peux vérifier la connectivité réseau avant de lancer une mise à jour (`ping` ou `nmcli`).
- 🧙‍♀️ Pour les adeptes de KDE Plasma, une version `kdialog` peut remplacer `echo` ou `read`.

---

## ⚠️ Avertissements rituels

- ❌ Ne jamais lancer ce genre de rituel automatiquement sur un **serveur critique** sans tests préalables.
- 🌀 Si tu utilises **Wayland**, ce type de magie visuelle peut ne pas fonctionner. Il faudra alors appeler les **démons systemd-timer**.

---

> *"Automatiser, c’est bien. Mais garder l'œil ouvert, c’est mieux."*

Que ton système reste stable, sage… et mis à jour.

✨ **Fin du rituel.**
