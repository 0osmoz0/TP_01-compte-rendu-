# TP1 – Configuration Réseau avec GNS3

## Équipe

| **Photo** | **Nom** | **GitHub** | **LinkedIn** |
|:---:|:---:|:---:|:---:|
| <img src="https://media.licdn.com/dms/image/v2/D4E35AQFSZbzOmWppmQ/profile-framedphoto-shrink_800_800/B4EZlO8KvFKoAg-/0/1757966019805?e=1759474800&v=beta&t=HJd1WHzfmilQ-Thc-PqNWM3TnV30VxeVV0fsgQXXTE8" width="64" height="64" style="border-radius: 50%;"> | **FARIA Anthony** | [![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=flat&logo=github)](https://github.com/Anthony-Faria-dos-santos) | [![LinkedIn](https://img.shields.io/badge/LinkedIn-Profile-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/anthony-faria-dos-santos/) |
| <img src="https://media.licdn.com/dms/image/v2/D4E35AQGAK7VJ8f5Y8Q/profile-framedphoto-shrink_400_400/B4EZkQ2TS1IwAk-/0/1756924294971?e=1759474800&v=beta&t=gOECMgZQSIjiqey5CndRGDSvl8-sU3v1P25ZIAuuNNw" width="64" height="64" style="border-radius: 50%;"> | **ARBARETAZ Quentin** | [![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=flat&logo=github)](https://github.com/CoDy-2224) | [![LinkedIn](https://img.shields.io/badge/LinkedIn-Profile-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/quentin-arbaretaz-technicien-reseaux/) |
| <img src="https://media.licdn.com/dms/image/v2/D4E03AQGHdkAhxCNjVw/profile-displayphoto-shrink_400_400/profile-displayphoto-shrink_400_400/0/1706858030136?e=1761782400&v=beta&t=JDE_bm8KRclKrAvf7pLllFoW47hSByShXb0uV4hLN-8" width="64" height="64" style="border-radius: 50%;"> | **BOUGARA Yani** | [![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=flat&logo=github)](https://github.com/yanibougara) | [![LinkedIn](https://img.shields.io/badge/LinkedIn-Profile-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/yani-bougara-15850224b/) |
| <img src="https://media.licdn.com/dms/image/v2/D4E03AQEvYhK-R9hC2A/profile-displayphoto-shrink_400_400/profile-displayphoto-shrink_400_400/0/1728762194206?e=1761782400&v=beta&t=_azOfyI8BZ6upUWR77NsWiClawd_98qToU3AQwkFY0M" width="64" height="64" style="border-radius: 50%;"> | **LEBEL Mathis** | [![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=flat&logo=github)](https://github.com/0osmoz0) | [![LinkedIn](https://img.shields.io/badge/LinkedIn-Profile-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/mathis-lebel-429114293/) |

---

## Table des matières

<details>
<summary><strong>Partie 1 : Création de la topologie GNS3 et configuration des interfaces</strong></summary>

- [1. Configuration des équipements](#1-configuration-des-équipements)
- [2. Tests de connectivité](#2-tests-de-connectivité)
</details>

<details>
<summary><strong>Partie 2 : Application de la passerelle</strong></summary>

- [1. Configuration](#1-configuration)
- [2. Validation](#2-validation)
</details>

<details>
<summary><strong>Partie 3 : Configuration VLAN</strong></summary>

- [Tâche 1 : Attribution des adresses IP](#tâche-1--attribution-des-adresses-ip)
- [Tâche 2 : Configuration de base du switch](#tâche-2--configuration-de-base-du-switch)
- [Tâche 3 : Création et configuration des VLANs](#tâche-3--création-et-configuration-des-vlans)
- [Tâche 4 : Vérification](#tâche-4--vérification)
- [Tâche 5 : Attribution des ports](#tâche-5--attribution-des-ports)
</details>

---

## Partie 1 : Créer la topologie GNS3 et configurer les interfaces réseaux

### 1. Configuration des équipements

Dans GNS3, une topologie de base a été mise en place comprenant :
- **Routeurs Cisco** : pour le routage inter-réseaux
- **Hôtes simulés (VPCS)** : pour simuler les postes clients
- **Switches** : pour la commutation locale

#### Analyse de l'état des connexions du routeur

Pour vérifier l'état des connexions du routeur et assurer leur stabilité et performance :

```bash
R1# show ip interface brief
FastEthernet0/0 unassigned YES unset administratively down down
FastEthernet2/0 unassigned YES unset administratively down down
FastEthernet3/0 unassigned YES unset administratively down down
FastEthernet3/1 unassigned YES unset administratively down down
```

**Analyse** : Toutes les interfaces sont en mode 'désactivé' dans la colonne 'Statut' en termes d'administration.

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

### 1. Configuration

#### Configuration IP et passerelle sur VPCS

```bash
VPCS> ip 192.168.1.2/24 192.168.1.1
```

#### Sauvegarde de la configuration du routeur

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

### Tâche 1 : Attribution des adresses IP

Chaque machine reçoit une adresse IP selon le plan d'adressage défini.

### Tâche 2 : Configuration de base du switch

#### Configuration des mots de passe

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

---

## Conclusion

Ce TP a permis d'acquérir les compétences suivantes :

### Objectifs atteints

| **Compétence** | **Description** | **Niveau acquis** |
|:---:|:---:|:---:|
| **Topologie réseau** | Création d'une architecture réseau complète dans GNS3 | Maîtrisé |
| **Configuration routeurs** | Configuration et activation des interfaces Cisco | Maîtrisé |
| **Tests de connectivité** | Utilisation des outils ping et traceroute | Maîtrisé |
| **Gestion des VLANs** | Création, configuration et attribution des VLANs | Maîtrisé |
| **Simulation Internet** | Mise en œuvre du NAT pour l'accès externe | Maîtrisé |

### Résultats obtenus

- **Connectivité validée** : Tous les tests de connectivité ont été réussis  
- **VLANs fonctionnels** : Séparation des réseaux étudiants et intervenants  
- **Configuration sauvegardée** : Persistance des configurations sur les équipements  
- **Architecture opérationnelle** : Topologie complète et fonctionnelle  

**Bilan :** Toutes les configurations et tests ont validé la bonne connectivité entre les équipements, démontrant une maîtrise des concepts de base du réseautage avec GNS3.

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
