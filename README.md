
# Installation Debian
Nom Machine: Jukebox
Domaine: <vide>
Utilisateur: jukebox
Miroir debian: debian.proxad.net
Logiciels: ne garder que `utilitaires usuels du système`
 

## Installation de SSH
Se connecter avec le compte root

apt-get install openssh-server

ifconfig

éditer /etc/ssh/sshd_config et mettre X11Forwarding no

## Installer sudo
apt-get install sudo
sudo usermod -aG sudo jukebox

# Ansible

Activer le virtualenv

## Activer l'accès root distant

Créer un fichier inventory/jukebox/host_vars/192.168.42.42/passwd.yml
et y mettre:

```yaml
ansible_ssh_pass: <MY_USER_PASSWORD>
ansible_become_pass: <MY_ROOT_PASSWORD>
```

```shell script
ansible-playbook -i inventory/jukebox allow_root.yml -vvv
```
## Installer le serveur

Créer un fichier inventory/root/host_vars/192.168.42.42/passwd.yml
et y mettre:

```yaml
ansible_ssh_pass: <MY_ROOT_PASSWORD>
```

Installation:
```shell script
ansible-playbook -i inventory/root jukebox.yaml -vvv
```

## Désactiver l'accès root distant

```shell script
ansible-playbook -i inventory/jukebox prohibit_root.yml -vvv
```
