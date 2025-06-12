# Organisation avancée de la configuration SSH 🔐
## 1. Modulariser avec Include
Depuis OpenSSH 7.3 (2016), tu peux inclure plusieurs fichiers de configuration dans le fichier ~/.ssh/config :

```ssh
# ~/.ssh/config
Include config.d/*.conf

Host client-A
  HostName server.client-a.com
  User userA
  IdentityFile ~/.ssh/keys/clientA_id_ed25519
```
- Include accepte des chemins avec glob (*) ou plusieurs fichiers.
- Place les directives Include en début de fichier pour éviter tout conflit 

## 2. Substitutions dynamiques
### a) Tokens internes %
- %d = répertoire home local
- %u = nom d’utilisateur local
- %h = host distant
- etc.

Exemples :

```ssh
IdentityFile %d/.ssh/id_rsa
ControlPath %d/.ssh/cm-%r@%h:%p
```
Ces tokens sont évalués à chaque utilisation.

### b) Variables d’environnement `${}`

Utilisables, depuis OpenSSH 8.4, via certaines directives (`IdentityFile`, `ControlPath`, `CertificateFile`, etc.) :

```ssh
IdentityFile ${HOME}/.ssh/cle_privee
```
Mais c'est limité à quelques directives ; en dehors cela ne fonctionne pas.

⚠️ Tu ne peux pas définir tes propres variables comme `$mykey = chemin`, SSH ne les interprète pas.

## 3. Astuces possibles
### a) Scripts + Include
Tu peux générer un fichier intermédiaire incluant des directives dynamiques via un script externe :

```bash
# host_config -> ~/.ssh/session-setup
echo "Hostname ${TARGET_HOST}"
echo "User ${TARGET_USER}"
```
Puis :

```ssh
Match host example.com
  Include ~/.ssh/session-setup
```
Cela permet une "configuration quasi-variable".

### b) Environnement distant (SetEnv / SendEnv)

Tu peux transmettre des variables d’environnement vers le SSH distant :

```ssh
Host serverX
  SetEnv FOO=bar
  SendEnv FOO
```
Le serveur doit accepter la variable via `AcceptEnv FOO` dans `/etc/ssh/sshd_config`.

Note : `TERM` peut même être manipulé via `SetEnv TERM=xterm` depuis OpenSSH 8.7.

## 4. Résumé des possibilités
| Objectif |	Possible directement ? |	Comment faire |
| :--- | :--- | :--- |
| Variables personnalisées dans config	| ❌ Non |	Générer dynamiquement un fichier via script et `Include` |
| Remplacer une clé par un "nom"	| ❌ Non |	Utiliser `${HOME}` ou tokens `%` |
| Automatiser les blocs par client	| ✅ Oui |	`Include` + fichiers dédiés |
| Passer des variables au serveur	| ✅ Oui |	`SetEnv` + `SendEnv` et `AcceptEnv` serveur |

Exemple complet
```ssh
# ~/.ssh/config
Include config.d/*.conf

Host *
  AddKeysToAgent yes
  IdentityFile ${HOME}/.ssh/id_rsa

Host client-A
  HostName a.example.com
  User userA
  IdentityFile ${HOME}/.ssh/keys/clientA_id_ed25519
  SetEnv PROJECT=A
  SendEnv PROJECT

Host client-B
  HostName b.example.org
  User userB
  IdentityFile ${HOME}/.ssh/keys/clientB_id_ed25519
  SetEnv PROJECT=B
  SendEnv PROJECT
```

Sur le serveur distant, ajoute dans `/etc/ssh/sshd_config` :

```ssh
AcceptEnv PROJECT
```

Ainsi, côté distant, `$PROJECT` sera défini automatiquement.

## ✅ En résumé
- Tu ne peux pas créer de variables personnalisées dans `ssh_config`.
- Utilise les directives `Include`, les tokens `%…`, et les variables `${HOME}` pour structurer ta config.
- Pour des configurations dynamiques, combine scripts + `Include`.
- Pour passer des variables d’environnement à la session distante, utilise `SetEnv` + `SendEnv` + `AcceptEnv`.
