# Consommer un rôle Ansible depuis GitHub

## 1. Installer Git sur l'Ansible Manager

Ansible Galaxy utilise Git pour récupérer les rôles hébergés sur GitHub.

```bash
sudo apt update
sudo apt install git -y
```

Vérifier l'installation :

```bash
git --version
```

## 2. Préparer le fichier requirements.yml

Créer un fichier `requirements.yml` dans le répertoire de travail du projet :

```bash
vim requirements.yml
```

Ajouter le contenu suivant :

```yaml
---
roles:
  - name: packages-install
    src: https://github.com/BAliani/packages-install.git
    version: main
```

**Explication :**
- `name` : nom du rôle à installer.
- `src` : URL du dépôt GitHub.
- `version` : branche Git à utiliser.

## 3. Installer le rôle avec Ansible Galaxy

Exécuter la commande suivante :

```bash
ansible-galaxy install -r requirements.yml
```

Le rôle sera installé par défaut dans :

```bash
~/.ansible/roles/
```

Vérifier sa présence :

```bash
ansible-galaxy role list
```

## 4. Préparer le playbook de test

Créer un fichier `playbook.yml` :

```bash
vim playbook.yml
```

Ajouter le contenu suivant :

```yaml
---
- name: Test external Ansible role
  hosts: all
  become: true

  roles:
    - packages-install
```

## 5. Exécuter le playbook

En supposant que l'inventaire Ansible s'appelle `inventory` :

```bash
ansible-playbook -i inventory playbook.yml
```

## 6. Vérifier le résultat

Sur les machines cibles, vérifier que les paquets sont installés :

```bash
dpkg -l | grep -E 'mysql-server|gedit'
```

Le résultat attendu est l'installation de MySQL Server et de Gedit sur les machines ciblées.

---

**À retenir :**

- `requirements.yml` : déclare les rôles externes nécessaires au projet.
- `ansible-galaxy install` : télécharge et installe les rôles.
- `playbook.yml` : consomme le rôle installé.
- `ansible-playbook` : exécute le scénario sur les machines cibles.

