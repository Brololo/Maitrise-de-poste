# Maîtrise de poste - Day 1

# Self-footprinting

## Host OS

🌞 Déterminer les principales informations de votre machine

1. nom de la machine

```
PS C:\Windows\system32> HOSTNAME.EXE
DESKTOP-94B3TGJ
```

2. OS et version

```
PS C:\Windows\system32> Get-ComputerInfo  | select windowsversion

WindowsVersion
--------------
1903
```

3. architecture processeur (32-bit, 64-bit, ARM, etc)

```
PS C:\Windows\system32> Get-WmiObject Win32_Processor


Caption           : Intel64 Family 6 Model 158 Stepping 10
DeviceID          : CPU0
Manufacturer      : GenuineIntel
MaxClockSpeed     : 2592
Name              : Intel(R) Core(TM) i7-9750H CPU @ 2.60GHz
SocketDesignation : U3E1
```

4. modèle du processeur

```
PS C:\Windows\system32> Get-WmiObject Win32_Processor


Caption           : Intel64 Family 6 Model 158 Stepping 10
DeviceID          : CPU0
Manufacturer      : GenuineIntel
MaxClockSpeed     : 2592
Name              : Intel(R) Core(TM) i7-9750H CPU @ 2.60GHz
SocketDesignation : U3E1
```

5. quantité RAM et modèle de la RAM

```
PS C:\Windows\system32> Get-WmiObject win32_physicalmemory


...
Capacity             : 17179869184
...
ConfiguredClockSpeed : 2666
ConfiguredVoltage    : 1200
...
```

## Devices

🌞 Trouver

- la marque et le modèle de votre processeur
  - identifier le nombre de processeurs, le nombre de coeur

```
PS C:\Users\Laurent> WMIC CPU Get DeviceID,NumberOfCores,NumberOfLogicalProcessors
DeviceID  NumberOfCores  NumberOfLogicalProcessors
CPU0      6              12

PS C:\Users\Laurent>
```

- si c'est un proc Intel, expliquer le nom du processeur (oui le nom veut dire quelque chose)

```
Intel
R : cette lettre est apparue avec la génération Haswell, elle désigne un modèle S avec une partie graphique intégrée (IGP) plus puissante

Core
T : il s’agit là d’une version « basse consommation »  avec des fréquences (de base et mode turbo) très inférieures au modèle standard.

M : processeurs Intel Core mobiles bicœurs assez puissants avec une fréquence plutôt élevée (pour un portable). Ils équipent généralement des portables « normaux » moyennement puissants comme le MacBook Pro 13 pouces et 13 pouces Retina par exemple.

i7-9750H CPU @ 2.60GHz

Core i7 : C’est le Processeur le plus haut de gamme pour le grand public. Il est disponible en 2, 4 ou 6 cœurs en fonction des versions. Il dispose d’une fréquence élevée (existe en version mobile et « bureaux »).

Famille de processeurs Intel® Core™ de 9e génération

Les trois chiffres suivants désignent la référence du processeur. Le plus important est le premier de ces trois chiffres, plus il est élevé, plus le processeur est haut de gamme, ce qui ne signifie pas toujours qu’il est plus puissant (les processeurs basse consommation des Ultrabooks ou des MacBook Air sont très couteux alors qu’il ne s’agit « que » de simples puces bicœurs)

H : Graphiques hautes performances
```

- la marque et le modèle :
  - de votre touchpad/trackpad
  - de vos enceintes intégrées

```
PS C:\Users\Laurent> Get-CimInstance win32_sounddevice

Manufacturer                Name                                                Status StatusInfo
------------                ----                                                ------ ----------
Realtek                     Realtek Audio                                       OK              3
Intel(R) Corporation        Technologie Intel® Smart Sound                      OK              3
Intel(R) Corporation        Son Intel(R) pour écrans                            OK              3
Valve Corporation Audio DDK Steam Streaming Microphone                          OK              3
Valve Corporation Audio DDK Steam Streaming Speakers                            OK              3
NVIDIA                      NVIDIA Virtual Audio Device (Wave Extensible) (WDM) OK              3
```

- de votre disque dur principal

```
PS C:\Users\Laurent> wmic diskdrive get Name,model,size
Model                      Name                Size
BC501 NVMe SK hynix 512GB  \\.\PHYSICALDRIVE0  512105932800
```

🌞 Disque dur

- identifier les différentes partitions de votre/vos disque(s) dur(s)
- déterminer le système de fichier de chaque partition
- expliquer la fonction de chaque partition

```
DriveLetter FriendlyName FileSystemType DriveType HealthStatus OperationalStatus SizeRemaining      Size
----------- ------------ -------------- --------- ------------ ----------------- -------------      ----
            DELLSUPPORT  NTFS           Fixed     Healthy      OK                    490.22 MB   1.29 GB Les bails de Dell j'imagine
            Image        NTFS           Fixed     Healthy      OK                     81.41 MB  16.57 GB ?
            WINRETOOLS   NTFS           Fixed     Healthy      OK                    523.66 MB    990 MB ?
C           OS           NTFS           Fixed     Healthy      OK                    129.19 GB 408.52 GB OS WINDOWS
D                        FAT32          Fixed     Healthy      OK                    196.64 MB 196.91 MB OS LINUX
```

## Network

🌞 Afficher la liste des cartes réseau de votre machine

- expliquer la fonction de chacune d'entre elles

```
PS C:\Users\Laurent> Get-NetAdapter | fl Name, InterfaceIndex


Name           : Wi-Fi || Ma carte wifi, j'ai pas ethernet :'[
InterfaceIndex : 16

Name           : VirtualBox Host-Only Network #2 réseau num 2 de virtualbox
InterfaceIndex : 15

Name           : VirtualBox Host-Only Network #3 réseau num 3 de virtualbox
InterfaceIndex : 11

Name           : VirtualBox Host-Only Network réseau num 1 de virtualbox
InterfaceIndex : 10

Name           : VirtualBox Host-Only Network #4 réseau num 4 de virtualbox
InterfaceIndex : 8
```

🌞 Lister tous les ports TCP et UDP en utilisation

- déterminer quel programme tourne derrière chacun des ports
- expliquer la fonction de chacun de ces programmes

## Users

🌞 Déterminer la liste des utilisateurs de la machine

- la liste **complète** des utilisateurs de la machine (je vous vois les Windowsiens...)
- déterminer le nom de l'utilisateur qui est full admin sur la machine

  - il existe toujours un utilisateur particulier qui a le droit de tout faire sur la machine

  ```
  PS C:\Users\Laurent> Get-LocalUser

    Name               Enabled Description
    ----               ------- -----------
    Administrateur     False   Compte d’utilisateur d’administration
    DefaultAccount     False   Compte utilisateur géré par le système.
    Invité             False   Compte d’utilisateur invité
    Laurent            True
    WDAGUtilityAccount False   Compte d’utilisateur géré et utilisé par le système pour les scénarios Windows Defender Application Guard.
    EntyAuthorityAdministrator le filou qui se cache.
  ```

## Processus

🌞 Déterminer la liste des processus de la machine

- je vous épargne l'explication de chacune des lignes, bien que ça serait pas plus mal...
- choisissez 5 services système et expliquer leur utilité
  - par "service système" j'entends des processus élémentaires au bon fonctionnement de la machine
  - sans eux, l'OS tel qu'on l'utilise n'existe pas
- déterminer les processus lancés par l'utilisateur qui est full admin sur la machine

# Scripting

Le scripting est une approche du développement qui consiste à automatiser de petites tâches simples, mais réalisées à intervalles réguliers ou un grand nombre de fois.

L'objectif de cette partie est de manipuler un langage de script natif à votre OS. Les principaux avantages d'utiliser un langage natif :

- bah... c'est natif ! Pas besoin d'installation
- le langage est mis à jour automatiquement, en même temps que le système
- le langage est rapide, car souvent bien intégré à l'environnement
- il permet d'accéder de façon simples aux ressources de l'OS (périphériques, processus, etc.) et de les manipuler de façon tout aussi aisée

🌞 Utiliser un langage de scripting natif à votre OS

- trouvez un langage natif par rapport à votre OS

  - s'il y en a plusieurs, expliquer le choix

  ```
  Powershell c'est le premier qui me vient à l'esprit.
  ```

- l'utiliser pour coder un script qui
  - affiche un résumé de l'OS
    - nom machine
    - IP principale
    - OS et version de l'OS
    - date et heure d'allumage
    - détermine si l'OS est à jour
    - Espace RAM utilisé / Espace RAM dispo
    - Espace disque utilisé / Espace disque dispo
  - liste les utilisateurs de la machine
  - calcule et affiche le temps de réponse moyen vers `8.8.8.8`
    - avec des `ping`

🐙 ajouter des fonctionnalités au script

- calcule et affiche le débit maximum en download et upload vers internet

---

🌞 Créer un deuxième script qui permet, en fonction d'arguments qui lui sont passés :

- exécuter une action
  - lock l'écran
  - éteindre le PC
- après X secondes

## Gestion de softs

Tous les OS modernes sont équipés ou peuvent être équipés d'un gestionnaire de paquets. Par exemple :

apt pour les GNU/Linux issus de Debian

dnf pour les GNU/Linux issus de RedHat

brew pour macOS

chocolatey pour Windows

_Expliquer l'intérêt de l'utilisation d'un gestionnaire de paquets:_
Un gestionnaire de paquet permet d'installer des logiciels, de les désinstaller et de les mettre à jour en une seule commande. Tous ces paquets sont centralisés dans un seul dépôt ce qui permet de ne pas parcourir le web pour une seule installation. Tous les logiciels sont dépourvus de malwares. Les dépendances sont automatiquement installées.

_Utiliser un gestionnaire de paquet propres à votre OS pour_:

```powershell
PS C:\WINDOWS\system32> choco list -l
Chocolatey v0.10.15
chocolatey 0.10.15
1 packages installed.
```

## Partage de fichiers

```
Je fais clic droit sur le document puis "partager" et il m'ouvre une fenêtre avec les outils qui peuvent utiliser le document dont le mail. Pas de samba je sais pas quoi là.
```

## Chiffrement de mails

```
Je vais sur mon mail je demande de créer un nouveau mail et je clique sur "chiffrer le mail".

Sinon on fait un algo de chiffrement et de déchiffrement "une clé" par exemple le chiffre de césar, on donne la clé au receveur et avant d'envoyer le message on l'envoi dans l'algo de chiffrement, puis on envoie le résultat et le receveur passe le message dans l'algo de déchiffrement et il obtient le message initial.

Pour encore plus de sécurité on peut utiliser une signature alors c'est dommage que sur le mail de l'école "l'admin" nous bloque l'accès et que sur mon mail perso je peux pas accéder aux options de S/MIME donc je peux pas le faire mais ça consiste à rajouter une signature comme si on faisait un chèque et ça prouve que le mail viens bien de nous.
```

---

LE RESTE VIENS DE IANIS DONC SI TU AS UNE IMPRESSION DE DEJA VU C EST NORMAL.

#### SSH

##### Client

_Générer une nouvelle paire de clés SSH:_

```powershell
ssh-keygen
```

_Déposer la clé nécessaire sur le serveur pour pouvoir vous y connecter:_
On met la clé publique dans un fichier ~/.ssh/authorized_keys.

_Expliquer tout ce qui est nécessaire pour se connecter avec un échange de clés, en ce qui concerne le client:_

- quelle(s) clé(s) sont générée(s) ? Comment ?
  Les clés privées et publiques sont générées grace à un chiffrement basé sur des clés asymétriques.
- quelle clé est déposée ? Pourquoi pas l'autre ?
  La clé déposée est la clé publique. On dépose celle ci parce que c'est la norme et pour une autre raison que tu avais expliqué dans un cours bonus mais j'ai zapé.
- à quoi ça sert précisément de déposer cette clé sur le serveur distant, qu'est-ce qu'il va pouvoir faire, précisément avec ?
  Cela permettra de déchiffrer les paquets envoyés par le client.
- dans quel fichier est stocké la clé ? Quelles permissions sur ce fichier ?
  ~/.ssh/authorized_keys avec les permissions -rw-------

_Le fingerprint SSH:_
C'est pour une identification / vérification facile de l'hôte auquel on se connecte

\*Créer un fichier ~/.ssh/config et y définir une connexion:

#### SSH avancé

##### SSH tunnels

_Mettez en place un serveur Web dans une VM:_

##### SSH jumps

#### Forwarding de ports at home
