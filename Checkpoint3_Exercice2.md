# Exercice 2 : Manipulations pratiques sur VM Linux (temps estimé : 2h30)

## Partie 1 : Gestion des utilisateurs
**Q.2.1.1 Sur le serveur, créer un compte pour ton usage personnel.**

![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20094847.png?raw=true)

**Q.2.1.2 Quelles préconisations proposes-tu concernant ce compte ?**
- S'ajouté au groupe sudo pour avoir les droit d'administration
- Avoir uin mod de passe fort et complexe 
- généré une paire de clé SSH pour un  authentification sécurisée sur les serveurs distants.

## Partie 2 : Configuration de SSH
**Q.2.2.1 Désactiver complètement l'accès à distance de l'utilisateur root.**

dans les fichié /etc/sshd/sshd_config

![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20095857.png?raw=true)

**Q.2.2.2 Autoriser l'accès à distance à ton compte personnel uniquement.**

dans les fichié /etc/sshd/sshd_config

![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20115444.png?raw=true)

**Q.2.2.3 Mettre en place une authentification par clé valide et désactiver l'authentification par mot de passe**


dans les fichié /etc/sshd/sshd_config
![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20101204.png?raw=true)

## Partie 3 : Analyse du stockage
**Q.2.3.1 Quels sont les systèmes de fichiers actuellement montés ?**

![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20101749.png?raw=true)

Depuis cette commande on peu lire que on a :

/dev/mapper/cp3--vg-root sur / (racine du système) Type : ext4

/dev/mdop1 sur /boot Type : ext2

devpts sur /dev/pts 

tmpfs sur /dev/shm 

hugetlbfs sur /dev/hugepages 

**Q.2.3.2 Quel type de système de stockage ils utilisent ?**
![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20101843.png?raw=true)

Avec cette commande on peu voir que l'on a :
Le disque principal (sda) utilise plusieurs systèmes de fichiers :

ext2 pour la partition /boot (mdop1)

LVM2_m (LVM2 member) pour mdop5,

ext4 pour le système de fichiers racine (/) monté sur cp3--vg-root

mdo fait pensé a une tentative de raid mais il y a pas de second disque 

Il y a également une partition de swap (cp3--vg-swap_1)

**Q.2.3.3 Ajouter un nouveau disque de 8,00 Gio au serveur et réparer le volume RAID**

```
# 1. Identifier le nouveau disque
lsblk
fdisk -l


# 2. Partitionner le nouveau disque si nécessaire
sudo fdisk /dev/sdX  # Remplacez X par la lettre appropriée
# Suivez les instructions pour créer une nouvelle partition

# 3. Identifier le volume RAID à réparer
cat /proc/mdstat

# 4. Ajouter le nouveau disque au RAID
sudo mdadm --add /dev/mdX /dev/sdXY  # Remplacez X et Y par les valeurs appropriées

# 5. Vérifier la reconstruction du RAID
watch cat /proc/mdstat

# 6. Une fois la reconstruction terminée, vérifier l'état du RAID
sudo mdadm --detail /dev/mdX

# 7. Mettre à jour la configuration RAID
sudo mdadm --detail --scan >> /etc/mdadm/mdadm.conf

# 8. Mettre à jour l'initramfs
sudo update-initramfs -u

# 9. Vérifier que le RAID démarre correctement au prochain redémarrage
sudo update-grub
```

![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20110742.png?raw=true)

on vois bien que avec cette procedure mon raid est bien actif 

**Q.2.3.4 Ajouter un nouveau volume logique LVM de 2 Gio qui servira à héberger des sauvegardes. Ce volume doit être monté automatiquement à chaque démarrage dans l'emplacement par défaut : /var/lib/bareos/storage.**

![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20111500.png?raw=true)
![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20112012.png?raw=true)
![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20112708.png?raw=true)
![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20113202.png?raw=true)

**Q.2.3.5 Combien d'espace disponible reste-t-il dans le groupe de volume ?**

![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20113344.png?raw=true)

on vois bien qu'il rest 1.79 G

## Partie 4 : Sauvegardes
**Q.2.4.1 Expliquer succinctement les rôles respectifs des 3 composants bareos installés sur la VM.**

Le Director (dir) est le cerveau qui coordonne tout.

Le Storage Daemon (sd) gère où et comment les données sont stockées.

Le File Daemon (fd) s'occupe de lire et écrire les données sur les systèmes à sauvegarder.


## Partie 5 : Filtrage et analyse réseau
Q.2.5.1 Quelles sont actuellement les règles appliquées sur Netfilter ?

![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20123040.png?raw=true)

**Q.2.5.2 Quels types de communications sont autorisées ?**
Toutes les connexions établies

Le trafic local

Les connexions SSH entrantes

Les pings (IPv4 et IPv6)

Probablement tout le trafic sortant


**Q.2.5.3 Quels types sont interdit ?**

Toute tentative de connexion qui ne correspond pas à une connexion déjà établie ou liée, et qui n'est pas du SSH ou de l'ICMP.

Tout paquet réseau qui serait considéré comme "invalide" par le système de suivi des connexions (conntrack).



**Q.2.5.4 Sur nftables, ajouter les règles nécessaires pour autoriser bareos à communiquer avec les clients bareos potentiellement présents sur l'ensemble des machines du réseau local sur lequel se trouve le serveur.**

on majoute les régles au fichié /etc/nftables.conf pour 
![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20124256.png?raw=true)

## Partie 6 : Analyse de logs
Q.2.6.1 Lister les 10 derniers échecs de connexion ayant eu lieu sur le serveur en indiquant pour chacun :

![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20133045.png?raw=true)


