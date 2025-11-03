<div align="center">

<img src="https://storage.letudiant.fr/osp/cards/316/LOGO-ESIEE-IT-Bee-blanc-fond-bleu-classique-241121064859.jpg" width="200" height="200" alt="ESIEE IT Logo">

# TP1 – Configuration Réseau avec GNS3

**Simulation et Configuration de Réseaux avec GNS3**

---

## Informations du TP

**Établissement :** ESIEE IT  
**Intervenant :** Yaya DOUMBIA  
**Promotion :** PO_TH6_DEV SOL NUM SEC L3C ALT 25-26  
**Année scolaire :** 2025-2026  

---

## Membres du Groupe

| **Photo** | **Nom** | **Prénom** | **GitHub** | **LinkedIn** |
|:---:|:---:|:---:|:---:|:---:|
| <img src="https://media.licdn.com/dms/image/v2/D4E35AQFSZbzOmWppmQ/profile-framedphoto-shrink_800_800/B4EZlO8KvFKoAg-/0/1757966019805?e=1762779600&v=beta&t=8FS1uTa39mLwLYkfbVJHbcqtv8tvlVdQXgFkt36wzJ0" width="64" height="64" style="border-radius: 50%;"> | **FARIA** | Anthony | [GitHub](https://github.com/Anthony-Faria-dos-santos) | [LinkedIn](https://www.linkedin.com/in/anthony-faria-dos-santos/) | |
| <img src="https://media.licdn.com/dms/image/v2/D4E03AQGHdkAhxCNjVw/profile-displayphoto-shrink_800_800/profile-displayphoto-shrink_800_800/0/1706858030136?e=1763596800&v=beta&t=pTEPh-7oxxlXHR73UxgkLTUTFp4Fi5Zma8mExZ7-xIY" width="64" height="64" style="border-radius: 50%;"> | **BOUGARA** | Yani | [GitHub](https://github.com/yanibougara) | [LinkedIn](https://www.linkedin.com/in/yani-bougara-15850224b/) |
| <img src="https://media.licdn.com/dms/image/v2/D4E03AQEvYhK-R9hC2A/profile-displayphoto-shrink_200_200/profile-displayphoto-shrink_200_200/0/1728762194206?e=1763596800&v=beta&t=dsrW9BQBnlMW1y5Y2rvKmuX9xGQCOoE7O5YMQxI9M7k" width="64" height="64" style="border-radius: 50%;"> | **LEBEL** | Mathis | [GitHub](https://github.com/0osmoz0) | [LinkedIn](https://www.linkedin.com/in/mathis-lebel-429114293/) |

---

*Compte-rendu de Travaux Pratiques - Configuration Réseau*

</div>


---

## Table des matières

<div align="center">

<details>
<summary><strong>Partie 1 : Création de la topologie GNS3 et configuration des interfaces</strong></summary>

- [1. Configuration des équipements](#1-configuration-des-équipements)
- [2. Tests de connectivité](#2-tests-de-connectivité)
</details>

<details>
<summary><strong>Partie 2 : Application de la passerelle et accès Internet</strong></summary>

- [1. Configuration](#1-configuration)
- [2. Accès à Internet depuis GNS3](#2-accès-à-internet-depuis-gns3)
- [3. Validation](#3-validation)
</details>

<details>
<summary><strong>Partie 3 : Configuration VLAN</strong></summary>

- [Tâche 1 : Attribution des adresses IP](#tâche-1--attribution-des-adresses-ip)
- [Tâche 2 : Configuration de base du switch](#tâche-2--configuration-de-base-du-switch)
- [Tâche 3 : Création et configuration des VLANs](#tâche-3--création-et-configuration-des-vlans)
- [Tâche 4 : Vérification](#tâche-4--vérification)
- [Tâche 5 : Attribution des ports](#tâche-5--attribution-des-ports)
- [Tâche 6 : Configuration du trunk et routage inter-VLAN](#tâche-6--configuration-du-trunk-et-routage-inter-vlan)
</details>

<details>
<summary><strong>Configuration des commutateurs Cisco IOU</strong></summary>

- [Problème : Licence IOU manquante](#problème--licence-iou-manquante)
- [Solution : Activation de la licence Cisco IOU](#solution--activation-de-la-licence-cisco-iou)
- [Configuration de la VM GNS3 et installation des images IOU](#configuration-de-la-vm-gns3-et-installation-des-images-iou)
</details>

<details>
<summary><strong>Ressources et Liens utiles</strong></summary>

- [Installation et Configuration GNS3](#installation-et-configuration-gns3)
- [Images et Équipements](#images-et-équipements)
</details>

</div>

---

## Partie 1 : Création de la topologie GNS3 et configuration des interfaces

<div align="center">

### Objectifs
- Créer une topologie réseau complète dans GNS3
- Configurer et activer les interfaces des équipements
- Tester la connectivité entre les éléments

</div>

### 1. Configuration des équipements

<div align="center">

**Architecture de la topologie mise en place :**

</div>

Dans GNS3, une topologie de base a été mise en place comprenant :

<div align="center">

| **Équipement** | **Rôle** | **Fonction** |
|:---:|:---:|:---:|
| **Routeurs Cisco** | Routage inter-réseaux | Acheminement des paquets entre réseaux |
| **Hôtes simulés (VPCS)** | Simulation de postes clients | Test de connectivité et services |
| **Switches** | Commutation locale | Connexion des équipements locaux |

</div>

#### Analyse de l'état des connexions du routeur

<div align="center">

**Vérification de l'état des interfaces réseau**

</div>

Pour vérifier l'état des connexions du routeur et assurer leur stabilité et performance :

```bash
R1# show ip interface brief
FastEthernet0/0 unassigned YES unset administratively down down
FastEthernet2/0 unassigned YES unset administratively down down
FastEthernet3/0 unassigned YES unset administratively down down
FastEthernet3/1 unassigned YES unset administratively down down
```

<div align="center">

> **Analyse** : Toutes les interfaces sont en mode 'désactivé' dans la colonne 'Statut' en termes d'administration.

</div>

#### Validation de la configuration initiale

```bash
R1# show running-config
```

Cette commande affiche une liste détaillée de tous les paramètres de configuration actuellement actifs, incluant :
- Les interfaces réseau
- Les protocoles de routage
- Les règles de pare-feu
- Les paramètres de sécurité

#### Configuration globale

La configuration globale concerne les réglages qui impactent l'ensemble du dispositif :

```bash
R1# configure terminal
# ou en abrégé
R1# conf t
```

**Éléments clés de la configuration globale :**
- **Interface de gestion** : Configuration des interfaces de gestion du routeur
- **Routage** : Configuration des protocoles (OSPF, BGP, RIP, MPLS)
- **Adresse IP et masque** : Configuration de l'interface de gestion
- **Sécurité** : Mesures de sécurité et règles de pare-feu

#### Activation d'une interface

```bash
R1(config)# interface FastEthernet0/0
R1(config-if)# no shutdown
```

#### Enregistrement de la configuration

```bash
# Méthode 1
R1# copy running-config startup-config

# Méthode 2 (alternative)
R1# write
```

#### Validation des paramètres stockés en RAM

```bash
R1# show running-config
```

### 2. Tests de connectivité

Une fois les interfaces configurées, la connectivité est testée avec les commandes suivantes :

| **Commande** | **Description** | **Utilisation** |
|:---:|:---:|:---:|
| `ping <adresse_IP>` | Test de connectivité directe | Vérifier la communication entre deux hôtes |
| `traceroute <adresse_IP>` | Analyse du chemin de routage | Identifier les routeurs intermédiaires |

## Partie 2 : Application de la passerelle et accès Internet

<div align="center">

### Objectifs
- Configurer les passerelles réseau
- Implémenter l'accès Internet via NAT
- Tester la connectivité externe

</div>

### 1. Configuration

#### Configuration IP et passerelle sur VPCS

<div align="center">

**Configuration des paramètres réseau des hôtes virtuels**

</div>

```bash
VPCS> ip 192.168.1.2/24 192.168.1.1
```

#### Sauvegarde de la configuration du routeur

<div align="center">

**Méthodes de sauvegarde des configurations**

</div>

```bash
# Méthode 1
R1# copy running-config startup-config

# Méthode 2 (alternative)
R1# write
```

### 2. Accès à Internet depuis GNS3

#### Le NAT (Network Address Translation)

La traduction d'adresses réseau (NAT) modifie les adresses IP et les ports source et destination. Son but est de :
- Réduire l'utilisation des adresses IP publiques IPv4
- Masquer les plages d'adresses des réseaux privés
- Permettre l'accès Internet aux appareils du LAN

**Fonctionnement du NAT :**
- Traduit les adresses IP privées en une unique adresse IP publique
- Permet aux appareils du LAN d'utiliser une même adresse IP publique
- Généralement effectué par des routeurs ou pare-feu

#### Topologie NAT et routeur

**Composants clés :**
- **NAT** : Traduit les adresses IP privées en adresse IP publique
- **Routeur** : Dispositif central reliant le LAN à Internet
- **Passerelle** : Gère le transfert des données entre réseaux

#### Tests de connectivité Internet

```bash
# Test de ping vers des serveurs publics
VPCS> ping google.com
VPCS> ping facebook.com

# Test de résolution DNS
VPCS> ping 8.8.8.8
```

### 3. Validation

Les tests suivants permettent de valider la configuration :

| **Test** | **Commande** | **Objectif** | **Résultat attendu** |
|:---:|:---:|:---:|:---:|
| **Ping passerelle** | `ping 192.168.1.1` | Vérifier la connectivité avec la passerelle | Réponse ICMP réussie |
| **Ping Internet** | `ping google.com` | Tester l'accès Internet via NAT | Réponse depuis serveur public |
| **Traceroute** | `traceroute <destination>` | Analyser le chemin de routage | Liste des routeurs intermédiaires |

## Partie 3 : Configuration VLAN

<div align="center">

### Objectifs
- Créer et configurer des VLANs
- Attribuer les ports aux VLANs appropriés
- Tester la segmentation réseau

</div>

### Tâche 1 : Attribution des adresses IP

<div align="center">

**Plan d'adressage des machines**

</div>

Chaque machine reçoit une adresse IP selon le plan d'adressage défini.

### Tâche 2 : Configuration de base du switch

#### Commutateurs Cisco recommandés pour les VLAN

<div align="center">

**Équipements recommandés pour la configuration VLAN**

</div>

Pour une configuration VLAN optimale, les commutateurs Cisco suivants sont recommandés :

<div align="center">

| **Type de commutateur** | **Version IOS** | **Utilisation** |
|:---:|:---:|:---:|
| **Commutateur L2** | 15.6.0.9S | Commutation de niveau 2, gestion des VLANs |
| **Commutateur L3** | L3-15 | Routage inter-VLAN, fonctionnalités avancées |

</div>

**Avantages des commutateurs recommandés :**
- **L2 15.6.0.9S** : Support complet des VLANs, STP, et fonctionnalités de commutation
- **L3-15** : Routage inter-VLAN, ACL, et fonctionnalités de sécurité avancées

#### Configuration des mots de passe

<div align="center">

**Sécurisation de l'accès au commutateur**

</div>

```bash
Switch(config)# enable secret class
Switch(config)# line console 0
Switch(config-line)# password cisco
Switch(config-line)# login
Switch(config)# line vty 0 4
Switch(config-line)# password cisco
Switch(config-line)# login
```

### Tâche 3 : Création et configuration des VLANs

#### Création des VLANs

```bash
# Création du VLAN 100 (Étudiants)
Switch(config)# vlan 100
Switch(config-vlan)# name etudiant

# Création du VLAN 200 (Intervenants)
Switch(config)# vlan 200
Switch(config-vlan)# name intervenant
```

#### Configuration de l'interface de gestion

```bash
Switch(config)# interface vlan 1
Switch(config-if)# ip address 192.168.1.254 255.255.255.0
Switch(config-if)# no shutdown
```

### Tâche 4 : Vérification

```bash
Switch# show vlan brief
```

### Tâche 5 : Attribution des ports

```bash
Switch(config)# interface fastEthernet 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 100
```

### Tâche 6 : Configuration du trunk et routage inter-VLAN

#### Principe du trunk

Un trunk est un lien qui transporte plusieurs VLANs entre un switch et un routeur (ou un autre switch). C'est ce lien qui permet au routeur IOU1 de recevoir les trames VLAN 100 et VLAN 200 pour effectuer le routage inter-VLAN.

<div align="center">

**Fonctionnement du trunk pour le routage inter-VLAN**

</div>

#### Étape 1 : Relier IOU1 au switch dans GNS3

Dans le schéma GNS3, connecter IOU1 au Switch1 avec un câble :

1. **Cliquer sur l'icône du câble** dans la barre d'outils
2. **Sélectionner les interfaces** à connecter
3. **Exemple de connexion** : Switch1 `e0/2` vers IOU1 `e0/0`

#### Étape 2 : Configurer le port du switch en trunk

Sur le switch, configurer le port en mode trunk pour transporter les VLANs :

```bash
Switch> enable
Switch# configure terminal
Switch(config)# interface e0/2
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 100,200
Switch(config-if)# exit
Switch(config)# exit
Switch# write
```

<div align="center">

**Explications des commandes**

</div>

| **Commande** | **Description** |
|:---:|:---:|
| `switchport mode trunk` | Active le mode trunk sur le port |
| `switchport trunk allowed vlan 100,200` | Autorise uniquement les VLAN 100 et 200 sur ce lien |

#### Étape 3 : Configurer les sous-interfaces sur IOU1 (le routeur)

Sur le routeur IOU1, créer une sous-interface pour chaque VLAN afin d'effectuer le routage inter-VLAN :

```bash
IOU1> enable
IOU1# configure terminal

IOU1(config)# interface e0/0.100
IOU1(config-subif)# encapsulation dot1Q 100
IOU1(config-subif)# ip address 172.16.100.1 255.255.255.0
IOU1(config-subif)# exit

IOU1(config)# interface e0/0.200
IOU1(config-subif)# encapsulation dot1Q 200
IOU1(config-subif)# ip address 172.16.200.1 255.255.255.0
IOU1(config-subif)# exit

IOU1(config)# exit
IOU1# write
```

<div align="center">

**Explications des configurations**

</div>

| **Élément** | **Description** |
|:---:|:---:|
| `e0/0.100` | Sous-interface virtuelle liée au VLAN 100 |
| `encapsulation dot1Q 100` | Cette interface traite les trames VLAN 100 |
| `ip address 172.16.100.1` | Adresse IP de la passerelle pour le VLAN 100 |
| `e0/0.200` | Sous-interface virtuelle liée au VLAN 200 |
| `encapsulation dot1Q 200` | Cette interface traite les trames VLAN 200 |
| `ip address 172.16.200.1` | Adresse IP de la passerelle pour le VLAN 200 |

**Architecture des sous-interfaces :**

- **VLAN 100** : Passerelle `172.16.100.1` (pour PC1)
- **VLAN 200** : Passerelle `172.16.200.1` (pour PC2)

#### Étape 4 : Configuration des PC et vérification de la connectivité

**Configuration des PC virtuels :**

<div align="center">

| **PC** | **Adresse IP** | **Masque** | **Passerelle** | **VLAN** |
|:---:|:---:|:---:|:---:|:---:|
| **PC1** | 172.16.100.10 | 255.255.255.0 | 172.16.100.1 | VLAN 100 |
| **PC2** | 172.16.200.10 | 255.255.255.0 | 172.16.200.1 | VLAN 200 |

</div>

**Commandes de configuration sur les VPCS :**

```bash
# Configuration PC1
VPCS> ip 172.16.100.10/24 172.16.100.1

# Configuration PC2
VPCS> ip 172.16.200.10/24 172.16.200.1
```

**Test de connectivité inter-VLAN :**

Depuis PC1, tester la communication avec PC2 :

```bash
# Depuis PC1
VPCS> ping 172.16.200.10
```

**Résultat attendu :** Si la configuration est correcte, PC1 devrait joindre PC2 via le routeur IOU1, démontrant que le routage inter-VLAN fonctionne correctement.

#### Architecture complète du routage inter-VLAN

<div align="center">

**Schéma de l'architecture**

</div>

```
PC1 (VLAN 100) → Switch (Port Access VLAN 100)
                      ↓
                  Trunk (VLAN 100, 200)
                      ↓
                 IOU1 (Sous-interfaces)
                      ↓
                  Trunk (VLAN 100, 200)
                      ↓
                Switch (Port Access VLAN 200)
                      ↓
             PC2 (VLAN 200)
```

**Flux de communication :**
1. PC1 (VLAN 100) envoie un paquet vers PC2 (VLAN 200)
2. Le switch transmet la trame VLAN 100 via le trunk vers IOU1
3. IOU1 reçoit la trame sur `e0/0.100`, route le paquet vers `e0/0.200`
4. IOU1 envoie la trame VLAN 200 via le trunk vers le switch
5. Le switch transmet la trame vers PC2 sur le VLAN 200

---

## Conclusion

<div align="center">

### Compétences acquises

</div>

Ce TP a permis d'acquérir les compétences suivantes :

### Objectifs atteints

<div align="center">

| **Compétence** | **Description** | **Niveau acquis** |
|:---:|:---:|:---:|
| **Topologie réseau** | Création d'une architecture réseau complète dans GNS3 | Maîtrisé |
| **Configuration routeurs** | Configuration et activation des interfaces Cisco | Maîtrisé |
| **Tests de connectivité** | Utilisation des outils ping et traceroute | Maîtrisé |
| **Gestion des VLANs** | Création, configuration et attribution des VLANs | Maîtrisé |
| **Simulation Internet** | Mise en œuvre du NAT pour l'accès externe | Maîtrisé |

</div>

### Résultats obtenus

<div align="center">

| **Résultat** | **Statut** | **Impact** |
|:---:|:---:|:---:|
| **Connectivité validée** | Réussi | Tous les tests de connectivité ont été réussis |
| **VLANs fonctionnels** | Réussi | Séparation des réseaux étudiants et intervenants |
| **Configuration sauvegardée** | Réussi | Persistance des configurations sur les équipements |
| **Architecture opérationnelle** | Réussi | Topologie complète et fonctionnelle |

</div>

<div align="center">

> **Bilan** : Toutes les configurations et tests ont validé la bonne connectivité entre les équipements, démontrant une maîtrise des concepts de base du réseautage avec GNS3.

</div>

---

## Ressources et Liens utiles

### Installation et Configuration GNS3

| **Plateforme** | **Lien** | **Description** |
|:---:|:---:|:---:|
| **VMware + GNS3** | [Installation étape par étape](https://www.it-connect.fr/introduction-a-gns3-installation-etape-par-etape-pour-bien-debuter/) | Guide complet d'installation |
| **VMware Player + GNS3** | [OpenClassrooms](https://openclassrooms.com/fr/courses/2581701-simulez-des-architectures-reseaux-avec-gns3/4823166-choisissez-et-installez-une-machine-virtuelle) | Cours OpenClassrooms |
| **Kali Linux** | [Installation GNS3](https://www.sysnettechsolutions.com/en/install-gns3-kali-linux/) | Installation sur Kali Linux |
| **VMware sur Kali** | [Installation VMware](https://www.sysnettechsolutions.com/en/install-vmware-kali-linux/) | VMware sur Kali Linux |

### Images et Équipements

| **Type d'équipement** | **Lien** | **Description** |
|:---:|:---:|:---:|
| **Images IOS Routeurs** | [Téléchargement gratuit](https://networkrare.com/free-download-cisco-ios-images-for-gns3-and-eve-ng/) | Images Cisco IOS pour GNS3 |
| **Commutateurs L2/L3** | [Images IOU/IOL](https://networkrare.com/download-cisco-iou-iol-images-gns3-gns3-iou-vm-oracle-virtual-box-l2-l3-cisco-switch-images/) | Images de commutateurs Cisco |
| **Commutateur L3** | [Configuration L3](https://yaser-rahmati.gitbook.io/gns3/l3-switching-simulation) | Ajout d'un commutateur de couche 3 |

---

## Configuration des commutateurs Cisco IOU

### Problème : Licence IOU manquante

Lors de l'utilisation des commutateurs Cisco IOU dans GNS3, vous pouvez rencontrer l'erreur suivante :

```
error while starting IOU1: Could not find an iourc file (IOU license), please configure an IOU license
```

Cette erreur indique qu'une licence IOU (IOS on Unix) est nécessaire pour utiliser les commutateurs Cisco dans GNS3.

### Solution : Activation de la licence Cisco IOU

#### Étape 1 : Ouvrir Putty

Putty est un émulateur de terminal qui permet de se connecter à des systèmes distants via SSH. Ouvrez Putty sur votre machine locale pour établir une connexion à la VM GNS3.

#### Étape 2 : Obtenir l'adresse IP et les identifiants

Récupérez l'adresse IP de la VM GNS3 depuis l'interface GNS3. Vous aurez également besoin du nom d'utilisateur et du mot de passe pour accéder à la VM.

![Connexion SSH avec GNS3](https://ccnaguru.com/wp-content/uploads/2024/02/coonect-ssh-with-gns3.png)

**Identifiants par défaut :**
- **Nom d'utilisateur** : `gns3`
- **Mot de passe** : `gns3`
- **Adresse IP** : Visible dans l'interface GNS3 (ex: 192.168.173.128)

#### Étape 3 : Connexion à la VM GNS3 via Putty

Entrez l'adresse IP de la VM GNS3 dans le champ "Host Name" de Putty. Cliquez sur "Open" pour établir la connexion SSH.

#### Étape 4 : Fournir les identifiants

Lorsque demandé, entrez le nom d'utilisateur et le mot de passe pour la VM GNS3.

#### Étape 5 : Sélectionner Shell et appuyer sur Entrée

Une fois connecté, sélectionnez l'option shell dans le menu et appuyez sur Entrée pour accéder à l'interface en ligne de commande de la VM GNS3.

#### Étape 6 : Télécharger le script Cisco IOU Keygen

Exécutez la commande suivante pour télécharger le script Cisco IOU Keygen :

```bash
wget http://www.ipvanquish.com/download/CiscoIOUKeygen3f.py
```

#### Étape 7 : Vérifier le fichier téléchargé

Exécutez la commande `ls` pour lister les fichiers du répertoire courant. Vérifiez que le fichier `CiscoIOUKeygen3f.py` est présent.

```bash
ls
```

#### Étape 8 : Exécuter le script Keygen

Exécutez la commande suivante pour exécuter le script Cisco IOU Keygen :

```bash
python3 CiscoIOUKeygen3f.py
```

![Exécution du script de génération de licence](https://ccnaguru.com/wp-content/uploads/2024/02/run-get-licence.png)

#### Étape 9 : Vérifier la sortie

Après avoir exécuté le script, exécutez à nouveau la commande `ls` pour vous assurer que le fichier `iourc.txt` a été généré.

```bash
ls
```

#### Étape 10 : Afficher les informations de licence

Utilisez la commande `cat iourc.txt` pour afficher le contenu du fichier `iourc.txt`. Copiez le texte affiché dans le terminal.

```bash
cat iourc.txt
```

**Exemple de sortie :**
```
[license]
gns3vm = 73635fd3b0a13ad0;
```

#### Étape 11 : Configurer les préférences GNS3

1. Ouvrez GNS3 et naviguez vers **Edit > Preferences**
2. Allez dans la section **"IOS on Unix"**
3. Collez le texte copié dans le champ approprié
4. Cliquez sur **"Apply"** puis **"OK"** pour sauvegarder les modifications

![Configuration de la licence IOU dans GNS3](https://ccnaguru.com/wp-content/uploads/2024/02/gns3-iourc-apply-1536x826.png)

### Résultat

En suivant ces instructions étape par étape, vous pouvez facilement activer une licence Cisco IOU dans GNS3 en utilisant Putty. Cela vous permet d'utiliser légalement les images Cisco IOU pour la simulation et les tests de réseau dans votre environnement GNS3.

---

## Configuration de la VM GNS3 et installation des images IOU

### Configuration de la VM GNS3

#### Étape 1 : Configuration des préférences GNS3 VM

Après avoir configuré la licence IOU, il est nécessaire de configurer la VM GNS3 dans les préférences :

1. **Ouvrir les préférences GNS3** : `Edit > Preferences`
2. **Sélectionner "GNS3 VM"** dans le panneau de gauche
3. **Activer la VM** : Cocher "Enable the GNS3 VM"
4. **Sélectionner la machine de virtualisation** : VMware, VirtualBox, ou QEMU
5. **Cliquer sur "Refresh"** pour détecter la VM
6. **La VM apparaît dans "VM name"** une fois détectée

#### Étape 2 : Configuration des actions de fermeture

Dans la section "Action when closing" :
- **Sélectionner "Keep GNS3 VM running"** pour maintenir la VM active
- Cela permet de conserver les configurations et d'éviter les redémarrages

#### Étape 3 : Vérification dans Server Summary

Une fois configurée, la VM GNS3 apparaît dans le **Server Summary** avec :
- **Statut** : Running
- **Utilisation CPU** : Affichage en temps réel
- **Utilisation RAM** : Monitoring des ressources

### Installation des images IOU

#### Étape 1 : Accès aux nouveaux équipements

1. **Naviguer vers "Browse all devices"**
2. **Cliquer sur "New device"**
3. **Sélectionner "Install new appliance from GNS3 server"**

#### Étape 2 : Importation des images IOU

1. **Rechercher "IOU L3"** dans la liste des appliances
2. **Sélectionner l'image IOU L3** appropriée
3. **Importer l'image .bin** dans la liste des images IOU
4. **Attendre la synchronisation** avec le serveur GNS3

#### Étape 3 : Configuration des interfaces réseau de la VM

**Configuration requise pour la VM GNS3 :**

| **Interface** | **Type** | **Configuration** | **Utilisation** |
|:---:|:---:|:---:|:---:|
| **Interface 1** | Host-only | Connexion directe avec l'hôte | Communication GNS3 |
| **Interface 2** | NAT | Accès Internet | Téléchargements et mises à jour |

**Avantages de cette configuration :**
- **Host-only** : Communication stable entre GNS3 et la VM
- **NAT** : Accès Internet pour les téléchargements d'images
- **Isolation** : Sécurité du réseau de simulation

### Séquence de démarrage optimale

#### Ordre de démarrage recommandé

1. **Démarrer la VM GNS3** en premier
2. **Lancer GNS3** sur l'hôte
3. **Vérifier la connexion** dans Server Summary
4. **Charger la topologie** existante ou créer une nouvelle
5. **Ajouter le routeur IOU** (IOU1) dans la topologie
6. **Configurer le NAT** pour l'accès Internet

#### Vérifications préalables

| **Élément** | **Statut requis** | **Vérification** |
|:---:|:---:|:---:|
| **VM GNS3** | Running | Server Summary |
| **Licence IOU** | Configurée | Preferences > IOS on Unix |
| **Images IOU** | Importées | Browse all devices |
| **Interfaces VM** | Configurées | Host-only + NAT |

### Utilisation des commutateurs IOU

#### Ajout d'un commutateur IOU dans la topologie

1. **Glisser-déposer** un commutateur IOU depuis la liste des équipements
2. **Configurer les interfaces** selon les besoins
3. **Connecter aux autres équipements** (routeurs, hôtes)
4. **Démarrer la simulation** pour tester la connectivité

#### Commandes de base pour les commutateurs IOU

```bash
# Accès au commutateur
Switch> enable
Switch# configure terminal

# Configuration des VLANs
Switch(config)# vlan 10
Switch(config-vlan)# name Sales
Switch(config-vlan)# exit

# Configuration des ports
Switch(config)# interface fastEthernet 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# no shutdown
```

### Résolution des problèmes courants

#### Problème : VM non détectée
- **Solution** : Vérifier que la VM est démarrée et que les interfaces réseau sont correctement configurées
- **Commande** : Redémarrer GNS3 et cliquer sur "Refresh"

#### Problème : Images IOU non disponibles
- **Solution** : Vérifier que la licence IOU est correctement configurée
- **Vérification** : Aller dans Preferences > IOS on Unix

#### Problème : Connexion instable
- **Solution** : Vérifier la configuration des interfaces réseau de la VM
- **Recommandation** : Utiliser Host-only pour la stabilité

### Commandes de référence

| **Commande** | **Description** | **Utilisation** |
|:---:|:---:|:---:|
| `wget <URL>` | Télécharge un fichier depuis une URL | Télécharger le script keygen |
| `ls` | Liste les fichiers du répertoire | Vérifier la présence des fichiers |
| `python3 <script>` | Exécute un script Python | Générer la licence IOU |
| `cat <fichier>` | Affiche le contenu d'un fichier | Voir le contenu de la licence |

---

## Glossaire

> **Lexique technique** - Définitions des termes utilisés dans ce document

<details>
<summary><strong>Équipements et Logiciels</strong></summary>

| **Terme** | **Définition** | **Contexte d'utilisation** |
|:---:|:---:|:---:|
| **GNS3** | Graphical Network Simulator-3, logiciel de simulation réseau permettant de créer des topologies complexes | Simulation et test de configurations réseau |
| **VPCS** | Virtual PC Simulator, simulateur d'ordinateurs virtuels dans GNS3 | Simulation d'hôtes clients |
| **Switch** | Équipement de commutation qui connecte plusieurs appareils sur un réseau local | Commutation de niveau 2 |
| **Routeur** | Équipement de routage qui connecte différents réseaux et achemine les paquets | Routage de niveau 3 |

</details>

<details>
<summary><strong>Technologies et Protocoles</strong></summary>

| **Terme** | **Définition** | **Contexte d'utilisation** |
|:---:|:---:|:---:|
| **VLAN** | Virtual Local Area Network, réseau local virtuel permettant de segmenter un réseau physique | Segmentation et isolation des réseaux |
| **FastEthernet** | Interface réseau Ethernet à 100 Mbps sur les équipements Cisco | Connexion physique des équipements |
| **NAT** | Network Address Translation, technique de traduction d'adresses IP | Traduction d'adresses |
| **ICMP** | Internet Control Message Protocol, protocole de contrôle et de diagnostic | Test de connectivité |

</details>

<details>
<summary><strong>Configuration et Interfaces</strong></summary>

| **Terme** | **Définition** | **Contexte d'utilisation** |
|:---:|:---:|:---:|
| **Interface** | Point de connexion physique ou logique d'un équipement réseau | Configuration des ports réseau |
| **Passerelle** | Routeur ou équipement permettant de faire transiter le trafic entre différents réseaux | Routage inter-réseaux |
| **Console** | Interface de configuration en ligne de commande des équipements réseau | Configuration locale |
| **VTY** | Virtual Teletype, lignes virtuelles pour l'accès distant aux équipements | Accès distant |
| **Access VLAN** | Mode d'attribution d'un port switch à un VLAN spécifique | Configuration des ports |

</details>

<details>
<summary><strong>Commandes et Tests</strong></summary>

| **Terme** | **Définition** | **Contexte d'utilisation** |
|:---:|:---:|:---:|
| **Ping** | Commande de test de connectivité réseau utilisant le protocole ICMP | Test de connectivité |
| **Traceroute** | Commande permettant de tracer le chemin emprunté par les paquets dans un réseau | Diagnostic de routage |

</details>
