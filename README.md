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

Vérifier sa présence :

```bash
ansible-galaxy role list
```

## 4. Préparer le playbook de test

Créer un fichier `playbook-install-pkg.yml` :

```bash
vim playbook-install-pkg.yml
```

## 5. Exécuter le playbook

En supposant que l'inventaire Ansible s'appelle `inventory` :

```bash
ansible-playbook -i inventory playbook-install-pkg.yml
```

