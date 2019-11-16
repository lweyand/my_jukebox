# Installation Debian

## Partitionnement

```
/dev/sda1 5M /boot/efi
/dev/sda2 5G /
/dev/sda3 <100% libre> lvm -> /dev/mapper/music_g-home_music
/dev/sda4 <100% taille RAM> swap
```

## fstab

La partition LVM est chargée dans */home/jukebox/Musique*.

```shell script
# / was on /dev/sda2 during installation
UUID=00f3c4f1-08e5-4e71-9481-5fef208cc21d /               ext4    errors=remount-ro 0       1
# /boot/efi was on /dev/sda1 during installation
UUID=634D-CCDD  /boot/efi       vfat    umask=0077      0       1
# /home/jukebox/Musique was on /dev/sda3 during installation
/dev/music_g/home_music /home/jukebox/Musique ext4 defaults 0 0
# swap was on /dev/sda4 during installation
UUID=116d6a14-f4da-4328-952c-e4ba206f7d9d none            swap    sw              0       0
```

## Installation de SSH
Se connecter avec le compte utilisateur

```shell script
sudo apt-get install openssh-server
```

## Installer sudo

```shell script
apt-get install sudo
sudo usermod -aG sudo jukebox
```


# Ansible

Activer le virtualenv

## Activer l'accès root distant

Créer un fichier inventory/jukebox/host_vars/jukebox_ip/passwd.yml
et y mettre:

```yaml
ansible_ssh_pass: <MY_USER_PASSWORD>
ansible_become_pass: <MY_ROOT_PASSWORD>
```

```shell script
ansible-playbook -i inventory/jukebox manage_ssh_root_access.yml --extra-vars '{"action":"allow"}'
```



## Installer le serveur

Créer un fichier inventory/root/host_vars/jukebox_ip/passwd.yml
et y mettre les mots de passe:

```yaml
ansible_ssh_pass: <MY_ROOT_PASSWORD>
samba_passwd: <SAMBA_PASSWORD_FOR_USER_JUKEBOX>
```

Installation:
```shell script
ansible-playbook -i inventory/root jukebox.yaml -vvv
```

## Désactiver l'accès root distant

```shell script
ansible-playbook -i inventory/jukebox manage_ssh_root_access.yml --extra-vars '{"action":"prohibit"}'
```

