<div align="center">

<img src="https://storage.letudiant.fr/osp/cards/316/LOGO-ESIEE-IT-Bee-blanc-fond-bleu-classique-241121064859.jpg" width="200" height="200" alt="ESIEE IT Logo">

# TP2 – Protocoles de Routage et MPLS VPN

**Documentation Complète - TP Protocoles de Routage et MPLS VPN**

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

*Compte-rendu de Travaux Pratiques - Protocoles de Routage et MPLS VPN*

</div>


---

## Table des matières

1. [Introduction](#introduction)
2. [Protocole OSPF](#protocole-ospf)
3. [Protocole BGP](#protocole-bgp)
4. [Protocole MPLS VPN](#protocole-mpls-vpn)
5. [Conclusion](#conclusion)

---

## Introduction

Ce travail pratique a pour objectif de vous familiariser avec trois protocoles essentiels dans les réseaux informatiques modernes :

- **OSPF** : Protocole de routage interne à état de liens
- **BGP** : Protocole de routage externe pour l'interconnexion entre systèmes autonomes
- **MPLS VPN** : Technologie de commutation par étiquettes pour créer des réseaux privés virtuels

### Prérequis

- Logiciel GNS3 installé
- Images IOS Cisco (routeurs)
- Connaissances de base en réseau IP
- Compétences en ligne de commande Cisco IOS

---

## Protocole OSPF

### 1. Présentation du Protocole OSPF

**OSPF (Open Shortest Path First)** est un protocole de routage dynamique à état de liens utilisé dans les réseaux IP. Il permet aux routeurs d'échanger des informations sur la topologie du réseau pour calculer les meilleures routes.

#### Avantages d'OSPF

- **Convergence rapide** : Détecte rapidement les changements de topologie
- **Scalabilité** : Supporte les grands réseaux grâce au système de zones
- **Routage optimal** : Utilise l'algorithme de Dijkstra pour calculer le chemin le plus court
- **Support du VLSM** : Permet une utilisation efficace des adresses IP
- **Pas de boucles de routage** : Architecture à état de liens
- **Flexibilité** : S'adapte à diverses topologies réseau

#### Inconvénients d'OSPF

- **Complexité** : Nécessite une expertise avancée pour la configuration
- **Charge CPU** : Peut consommer beaucoup de ressources processeur
- **Configuration initiale** : Demande une planification minutieuse
- **Évolutivité limitée** : Moins adapté aux très grands réseaux comme Internet

### 2. Topologie OSPF

```
        PC1                    PC2

         |                      |

    [R1]----[R2]----[R3]----[R4]

    

    Configuration suggérée :

    - Réseau entre R1-R2 : 10.0.12.0/24

    - Réseau entre R2-R3 : 10.0.23.0/24

    - Réseau entre R3-R4 : 10.0.34.0/24

    - Réseau PC1 : 192.168.1.0/24

    - Réseau PC2 : 192.168.2.0/24

```

### 3. Configuration Détaillée OSPF

#### Configuration du Routeur R1

```cisco
! Entrer en mode configuration
Router> enable
Router# configure terminal

! Configuration du hostname
Router(config)# hostname R1

! Configuration des interfaces
R1(config)# interface FastEthernet0/0
R1(config-if)# description Connexion vers PC1
R1(config-if)# ip address 192.168.1.254 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

R1(config)# interface FastEthernet0/1
R1(config-if)# description Connexion vers R2
R1(config-if)# ip address 10.0.12.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

! Configuration d'une interface Loopback (pour l'ID du routeur)
R1(config)# interface Loopback0
R1(config-if)# ip address 1.1.1.1 255.255.255.255
R1(config-if)# exit

! Activation du protocole OSPF
R1(config)# router ospf 1
R1(config-router)# router-id 1.1.1.1
R1(config-router)# network 192.168.1.0 0.0.0.255 area 0
R1(config-router)# network 10.0.12.0 0.0.0.255 area 0
R1(config-router)# network 1.1.1.1 0.0.0.0 area 0
R1(config-router)# exit

! Sauvegarde de la configuration
R1(config)# exit
R1# write memory
```

#### Configuration du Routeur R2

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname R2

! Configuration des interfaces
R2(config)# interface FastEthernet0/0
R2(config-if)# description Connexion vers R1
R2(config-if)# ip address 10.0.12.2 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# exit

R2(config)# interface FastEthernet0/1
R2(config-if)# description Connexion vers R3
R2(config-if)# ip address 10.0.23.1 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# exit

! Configuration Loopback
R2(config)# interface Loopback0
R2(config-if)# ip address 2.2.2.2 255.255.255.255
R2(config-if)# exit

! Configuration OSPF
R2(config)# router ospf 1
R2(config-router)# router-id 2.2.2.2
R2(config-router)# network 10.0.12.0 0.0.0.255 area 0
R2(config-router)# network 10.0.23.0 0.0.0.255 area 0
R2(config-router)# network 2.2.2.2 0.0.0.0 area 0
R2(config-router)# exit

R2(config)# exit
R2# write memory
```

#### Configuration du Routeur R3

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname R3

! Configuration des interfaces
R3(config)# interface FastEthernet0/0
R3(config-if)# description Connexion vers R2
R3(config-if)# ip address 10.0.23.2 255.255.255.0
R3(config-if)# no shutdown
R3(config-if)# exit

R3(config)# interface FastEthernet0/1
R3(config-if)# description Connexion vers R4
R3(config-if)# ip address 10.0.34.1 255.255.255.0
R3(config-if)# no shutdown
R3(config-if)# exit

! Configuration Loopback
R3(config)# interface Loopback0
R3(config-if)# ip address 3.3.3.3 255.255.255.255
R3(config-if)# exit

! Configuration OSPF
R3(config)# router ospf 1
R3(config-router)# router-id 3.3.3.3
R3(config-router)# network 10.0.23.0 0.0.0.255 area 0
R3(config-router)# network 10.0.34.0 0.0.0.255 area 0
R3(config-router)# network 3.3.3.3 0.0.0.0 area 0
R3(config-router)# exit

R3(config)# exit
R3# write memory
```

#### Configuration du Routeur R4

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname R4

! Configuration des interfaces
R4(config)# interface FastEthernet0/0
R4(config-if)# description Connexion vers R3
R4(config-if)# ip address 10.0.34.2 255.255.255.0
R4(config-if)# no shutdown
R4(config-if)# exit

R4(config)# interface FastEthernet0/1
R4(config-if)# description Connexion vers PC2
R4(config-if)# ip address 192.168.2.254 255.255.255.0
R4(config-if)# no shutdown
R4(config-if)# exit

! Configuration Loopback
R4(config)# interface Loopback0
R4(config-if)# ip address 4.4.4.4 255.255.255.255
R4(config-if)# exit

! Configuration OSPF
R4(config)# router ospf 1
R4(config-router)# router-id 4.4.4.4
R4(config-router)# network 10.0.34.0 0.0.0.255 area 0
R4(config-router)# network 192.168.2.0 0.0.0.255 area 0
R4(config-router)# network 4.4.4.4 0.0.0.0 area 0
R4(config-router)# exit

R4(config)# exit
R4# write memory
```

### 4. Vérification de la Configuration OSPF

#### Sur chaque routeur, exécuter les commandes suivantes :

```cisco
! Vérifier les voisins OSPF
R1# show ip ospf neighbor

! Résultat attendu :
! Neighbor ID     Pri   State           Dead Time   Address         Interface
! 2.2.2.2          1    FULL/DR         00:00:35    10.0.12.2       FastEthernet0/1

! Vérifier les interfaces OSPF
R1# show ip ospf interface brief

! Vérifier la table de routage
R1# show ip route

! Résultat attendu : routes OSPF marquées avec "O"
! O    192.168.2.0/24 [110/X] via 10.0.12.2, FastEthernet0/1

! Vérifier les détails OSPF
R1# show ip ospf

! Vérifier la base de données OSPF
R1# show ip ospf database
```

### 5. Tests de Connectivité OSPF

```cisco
! Test ping depuis R1 vers R4
R1# ping 4.4.4.4

! Test ping vers le réseau distant
R1# ping 192.168.2.254

! Tracer le chemin
R1# traceroute 192.168.2.254

! Test ping depuis PC1 vers PC2
PC1> ping 192.168.2.1
```

### 6. Dépannage OSPF

**Problèmes courants et solutions :**

1. **Les voisins OSPF ne s'établissent pas**

   ```cisco
   ! Vérifier que les interfaces sont actives
   R1# show ip interface brief
   
   ! Vérifier les paramètres OSPF
   R1# show ip ospf interface
   
   ! Vérifier les timers OSPF (doivent correspondre)
   R1# show ip ospf interface FastEthernet0/1
   ```

2. **Les routes n'apparaissent pas**

   ```cisco
   ! Vérifier les annonces réseau
   R1# show running-config | section router ospf
   
   ! Vérifier les aires OSPF
   R1# show ip ospf
   ```

3. **Réinitialiser le processus OSPF**

   ```cisco
   R1# clear ip ospf process
   ```

---

## Protocole BGP

### 1. Présentation du Protocole BGP

**BGP (Border Gateway Protocol)** est le protocole de routage externe utilisé sur Internet. Il permet l'échange d'informations de routage entre systèmes autonomes (AS).

#### Concepts Clés

- **Système Autonome (AS)** : Ensemble de réseaux sous une même administration
- **eBGP** : BGP externe (entre différents AS)
- **iBGP** : BGP interne (au sein d'un même AS)
- **Attributs BGP** : AS-PATH, NEXT-HOP, LOCAL-PREF, etc.

#### Avantages de BGP

- **Flexibilité** : Politiques de routage personnalisables
- **Support CIDR** : Agrégation de préfixes pour réduire la taille des tables
- **Contrôle manuel** : Configuration précise des connexions entre pairs
- **Fiabilité** : Utilise TCP comme protocole de transport

#### Inconvénients de BGP

- **Complexité** : Protocole difficile à configurer et maintenir
- **Scalabilité** : Taille croissante des tables de routage BGP
- **Contrôle limité** : Influence limitée sur les décisions des autres AS
- **Risques de mauvaise annonce** : Peut rendre des préfixes inaccessibles

### 2. Topologie BGP

```
    AS 100           AS 200           AS 300

      |                |                |

    [R1]----------[R2]----------[R3]

    

    Configuration suggérée :

    - R1 : AS 100, IP 10.0.12.1/24

    - R2 : AS 200, IP 10.0.12.2/24 et 10.0.23.1/24

    - R3 : AS 300, IP 10.0.23.2/24

    

    - Réseau annoncé par AS100 : 192.168.1.0/24

    - Réseau annoncé par AS200 : 192.168.2.0/24

    - Réseau annoncé par AS300 : 192.168.3.0/24

```

### 3. Configuration Détaillée BGP

#### Configuration du Routeur R1 (AS 100)

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname R1

! Configuration des interfaces
R1(config)# interface FastEthernet0/0
R1(config-if)# description Connexion vers réseau local
R1(config-if)# ip address 192.168.1.254 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

R1(config)# interface FastEthernet0/1
R1(config-if)# description Connexion vers R2 (AS 200)
R1(config-if)# ip address 10.0.12.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

! Configuration Loopback
R1(config)# interface Loopback0
R1(config-if)# ip address 1.1.1.1 255.255.255.255
R1(config-if)# exit

! Configuration BGP
R1(config)# router bgp 100
R1(config-router)# bgp router-id 1.1.1.1
R1(config-router)# neighbor 10.0.12.2 remote-as 200
R1(config-router)# neighbor 10.0.12.2 description R2-AS200
R1(config-router)# network 192.168.1.0 mask 255.255.255.0
R1(config-router)# network 1.1.1.1 mask 255.255.255.255
R1(config-router)# exit

R1(config)# exit
R1# write memory
```

#### Configuration du Routeur R2 (AS 200)

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname R2

! Configuration des interfaces
R2(config)# interface FastEthernet0/0
R2(config-if)# description Connexion vers R1 (AS 100)
R2(config-if)# ip address 10.0.12.2 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# exit

R2(config)# interface FastEthernet0/1
R2(config-if)# description Connexion vers R3 (AS 300)
R2(config-if)# ip address 10.0.23.1 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# exit

R2(config)# interface FastEthernet1/0
R2(config-if)# description Connexion vers réseau local
R2(config-if)# ip address 192.168.2.254 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# exit

! Configuration Loopback
R2(config)# interface Loopback0
R2(config-if)# ip address 2.2.2.2 255.255.255.255
R2(config-if)# exit

! Configuration BGP
R2(config)# router bgp 200
R2(config-router)# bgp router-id 2.2.2.2
R2(config-router)# neighbor 10.0.12.1 remote-as 100
R2(config-router)# neighbor 10.0.12.1 description R1-AS100
R2(config-router)# neighbor 10.0.23.2 remote-as 300
R2(config-router)# neighbor 10.0.23.2 description R3-AS300
R2(config-router)# network 192.168.2.0 mask 255.255.255.0
R2(config-router)# network 2.2.2.2 mask 255.255.255.255
R2(config-router)# exit

R2(config)# exit
R2# write memory
```

#### Configuration du Routeur R3 (AS 300)

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname R3

! Configuration des interfaces
R3(config)# interface FastEthernet0/0
R3(config-if)# description Connexion vers R2 (AS 200)
R3(config-if)# ip address 10.0.23.2 255.255.255.0
R3(config-if)# no shutdown
R3(config-if)# exit

R3(config)# interface FastEthernet0/1
R3(config-if)# description Connexion vers réseau local
R3(config-if)# ip address 192.168.3.254 255.255.255.0
R3(config-if)# no shutdown
R3(config-if)# exit

! Configuration Loopback
R3(config)# interface Loopback0
R3(config-if)# ip address 3.3.3.3 255.255.255.255
R3(config-if)# exit

! Configuration BGP
R3(config)# router bgp 300
R3(config-router)# bgp router-id 3.3.3.3
R3(config-router)# neighbor 10.0.23.1 remote-as 200
R3(config-router)# neighbor 10.0.23.1 description R2-AS200
R3(config-router)# network 192.168.3.0 mask 255.255.255.0
R3(config-router)# network 3.3.3.3 mask 255.255.255.255
R3(config-router)# exit

R3(config)# exit
R3# write memory
```

### 4. Vérification de la Configuration BGP

```cisco
! Vérifier le résumé BGP
R1# show ip bgp summary

! Résultat attendu :
! Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
! 10.0.12.2       4   200      15      15        5    0    0 00:10:25        2

! Vérifier la table BGP
R1# show ip bgp

! Vérifier les voisins BGP
R1# show ip bgp neighbors

! Vérifier un préfixe spécifique
R1# show ip bgp 192.168.3.0

! Vérifier la table de routage
R1# show ip route bgp

! Résultat attendu : routes BGP marquées avec "B"
! B    192.168.2.0/24 [20/0] via 10.0.12.2, 00:10:25
! B    192.168.3.0/24 [20/0] via 10.0.12.2, 00:10:25
```

### 5. Tests de Connectivité BGP

```cisco
! Test ping depuis R1 vers les réseaux distants
R1# ping 192.168.2.254
R1# ping 192.168.3.254
R1# ping 2.2.2.2
R1# ping 3.3.3.3

! Tracer le chemin
R1# traceroute 192.168.3.254
```

### 6. Configuration Avancée BGP (Optionnel)

#### Agrégation de Routes

```cisco
R1(config)# router bgp 100
R1(config-router)# aggregate-address 192.168.0.0 255.255.252.0 summary-only
```

#### Filtrage avec Prefix-List

```cisco
! Créer une prefix-list
R1(config)# ip prefix-list ALLOW_SPECIFIC permit 192.168.1.0/24
R1(config)# ip prefix-list ALLOW_SPECIFIC deny 0.0.0.0/0 le 32

! Appliquer au voisin
R1(config)# router bgp 100
R1(config-router)# neighbor 10.0.12.2 prefix-list ALLOW_SPECIFIC out
```

#### Route-Map pour Modifier les Attributs

```cisco
! Créer une route-map
R1(config)# route-map SET_LOCAL_PREF permit 10
R1(config-route-map)# set local-preference 200
R1(config-route-map)# exit

! Appliquer au voisin
R1(config)# router bgp 100
R1(config-router)# neighbor 10.0.12.2 route-map SET_LOCAL_PREF in
```

---

## Protocole MPLS VPN

### 1. Présentation du Protocole MPLS

**MPLS (Multiprotocol Label Switching)** est une technologie de commutation par étiquettes qui améliore l'efficacité et les performances des réseaux IP. Elle permet de créer des chemins virtuels définis par des labels plutôt que par des adresses IP.

#### Concepts Clés

**VRF (Virtual Routing and Forwarding)**

- Table de routage virtuelle spécifique à un VPN
- Isole le trafic de différents clients
- Chaque interface PE vers un site client est associée à une VRF

**Types de Routeurs**

1. **Routeur CE (Customer Edge)**
   - Point d'entrée du client vers le backbone
   - Routage IP traditionnel
   - Aucune connaissance MPLS requise

2. **Routeur P (Provider)**
   - Cœur du backbone MPLS
   - Commutation des paquets MPLS
   - Pas de connaissance des VPN

3. **Routeur PE (Provider Edge)**
   - Périphérie du réseau backbone
   - Gestion des VPN et des VRF
   - Connecté aux routeurs CE

#### Avantages de MPLS VPN

- **Sécurité renforcée** : Données non transmises sur Internet
- **Hautes performances** : Faible latence et débit élevé
- **Compatibilité** : S'intègre aux réseaux IP existants
- **Multi-tenant** : Support de plusieurs clients sur un même réseau

#### Inconvénients de MPLS VPN

- **Coût élevé** : Infrastructure et abonnements coûteux
- **Complexité** : Nécessite une expertise spécialisée
- **Configuration** : Planification et maintenance complexes

### 2. Topologie MPLS VPN

```
Site Client A          Backbone MPLS          Site Client B

   (VPN A)               Provider                (VPN A)

     

 PC1----[CE1(R4)]----[PE1(R1)]----[P(R2)]----[PE2(R3)]----[CE2(R7)]----PC2

                          |                         |

                      [CE3(R5)]                [CE4(R6)]

                          |                         |

                         PC3                       PC4

                    Site Client C             Site Client C

                      (VPN B)                   (VPN B)

Configuration :

- R1 (PE1) : 10.1.14.1, 10.1.12.1, Loopback 1.1.1.1

- R2 (P)   : 10.1.12.2, 10.1.23.1, Loopback 2.2.2.2

- R3 (PE2) : 10.1.23.2, 10.1.36.1, 10.1.37.1, Loopback 3.3.3.3

- R4 (CE1) : 10.1.14.4, 172.16.4.1 (réseau client)

- R5 (CE3) : 10.1.15.5, 172.16.5.1 (réseau client)

- R6 (CE4) : 10.1.36.6, 172.16.6.1 (réseau client)

- R7 (CE2) : 10.1.37.7, 172.16.7.1 (réseau client)

```

### 3. Configuration Détaillée MPLS VPN

#### Étape 1 : Configuration de base et OSPF dans le backbone

##### Configuration du Routeur PE1 (R1)

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname PE1

! Configuration des interfaces backbone
PE1(config)# interface Loopback0
PE1(config-if)# ip address 1.1.1.1 255.255.255.255
PE1(config-if)# exit

PE1(config)# interface FastEthernet0/0
PE1(config-if)# description Vers P (R2)
PE1(config-if)# ip address 10.1.12.1 255.255.255.0
PE1(config-if)# no shutdown
PE1(config-if)# exit

! Configuration OSPF dans le backbone
PE1(config)# router ospf 1
PE1(config-router)# router-id 1.1.1.1
PE1(config-router)# network 1.1.1.1 0.0.0.0 area 0
PE1(config-router)# network 10.1.12.0 0.0.0.255 area 0
PE1(config-router)# exit

PE1(config)# exit
PE1# write memory
```

##### Configuration du Routeur P (R2)

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname P

! Configuration des interfaces
P(config)# interface Loopback0
P(config-if)# ip address 2.2.2.2 255.255.255.255
P(config-if)# exit

P(config)# interface FastEthernet0/0
P(config-if)# description Vers PE1 (R1)
P(config-if)# ip address 10.1.12.2 255.255.255.0
P(config-if)# no shutdown
P(config-if)# exit

P(config)# interface FastEthernet0/1
P(config-if)# description Vers PE2 (R3)
P(config-if)# ip address 10.1.23.1 255.255.255.0
P(config-if)# no shutdown
P(config-if)# exit

! Configuration OSPF
P(config)# router ospf 1
P(config-router)# router-id 2.2.2.2
P(config-router)# network 2.2.2.2 0.0.0.0 area 0
P(config-router)# network 10.1.12.0 0.0.0.255 area 0
P(config-router)# network 10.1.23.0 0.0.0.255 area 0
P(config-router)# exit

P(config)# exit
P# write memory
```

##### Configuration du Routeur PE2 (R3)

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname PE2

! Configuration des interfaces backbone
PE2(config)# interface Loopback0
PE2(config-if)# ip address 3.3.3.3 255.255.255.255
PE2(config-if)# exit

PE2(config)# interface FastEthernet0/0
PE2(config-if)# description Vers P (R2)
PE2(config-if)# ip address 10.1.23.2 255.255.255.0
PE2(config-if)# no shutdown
PE2(config-if)# exit

! Configuration OSPF dans le backbone
PE2(config)# router ospf 1
PE2(config-router)# router-id 3.3.3.3
PE2(config-router)# network 3.3.3.3 0.0.0.0 area 0
PE2(config-router)# network 10.1.23.0 0.0.0.255 area 0
PE2(config-router)# exit

PE2(config)# exit
PE2# write memory
```

#### Étape 2 : Activation de MPLS

##### Sur PE1 (R1)

```cisco
PE1# configure terminal

! Activer CEF (Cisco Express Forwarding) - prérequis pour MPLS
PE1(config)# ip cef

! Configuration MPLS globale
PE1(config)# mpls label protocol ldp
PE1(config)# mpls ldp router-id Loopback0 force

! Activer MPLS sur les interfaces backbone
PE1(config)# interface FastEthernet0/0
PE1(config-if)# mpls ip
PE1(config-if)# exit

PE1(config)# exit
PE1# write memory
```

##### Sur P (R2)

```cisco
P# configure terminal

! Activer CEF
P(config)# ip cef

! Configuration MPLS globale
P(config)# mpls label protocol ldp
P(config)# mpls ldp router-id Loopback0 force

! Activer MPLS sur toutes les interfaces backbone
P(config)# interface FastEthernet0/0
P(config-if)# mpls ip
P(config-if)# exit

P(config)# interface FastEthernet0/1
P(config-if)# mpls ip
P(config-if)# exit

P(config)# exit
P# write memory
```

##### Sur PE2 (R3)

```cisco
PE2# configure terminal

! Activer CEF
PE2(config)# ip cef

! Configuration MPLS globale
PE2(config)# mpls label protocol ldp
PE2(config)# mpls ldp router-id Loopback0 force

! Activer MPLS sur les interfaces backbone
PE2(config)# interface FastEthernet0/0
PE2(config-if)# mpls ip
PE2(config-if)# exit

PE2(config)# exit
PE2# write memory
```

#### Vérification MPLS

```cisco
! Vérifier les voisins LDP
PE1# show mpls ldp neighbor

! Résultat attendu :
!     Peer LDP Ident: 2.2.2.2:0; Local LDP Ident 1.1.1.1:0
!         TCP connection: 2.2.2.2.646 - 1.1.1.1.11005
!         State: Oper; Msgs sent/rcvd: 8/8; Downstream
!         Up time: 00:05:23

! Vérifier les bindings MPLS
PE1# show mpls ldp bindings

! Vérifier la table MPLS forwarding
PE1# show mpls forwarding-table

! Vérifier les interfaces MPLS
PE1# show mpls interfaces
```

#### Étape 3 : Configuration des VRF

##### Sur PE1 (R1) - Création des VRF

```cisco
PE1# configure terminal

! Créer VRF pour le Client A
PE1(config)# ip vrf CLIENT_A
PE1(config-vrf)# rd 100:1
PE1(config-vrf)# route-target export 100:1
PE1(config-vrf)# route-target import 100:1
PE1(config-vrf)# exit

! Créer VRF pour le Client B
PE1(config)# ip vrf CLIENT_B
PE1(config-vrf)# rd 100:2
PE1(config-vrf)# route-target export 100:2
PE1(config-vrf)# route-target import 100:2
PE1(config-vrf)# exit

! Associer les interfaces CE aux VRF
PE1(config)# interface FastEthernet1/0
PE1(config-if)# description Vers CE1 (R4) - Client A
PE1(config-if)# ip vrf forwarding CLIENT_A
PE1(config-if)# ip address 10.1.14.1 255.255.255.0
PE1(config-if)# no shutdown
PE1(config-if)# exit

PE1(config)# interface FastEthernet1/1
PE1(config-if)# description Vers CE3 (R5) - Client B
PE1(config-if)# ip vrf forwarding CLIENT_B
PE1(config-if)# ip address 10.1.15.1 255.255.255.0
PE1(config-if)# no shutdown
PE1(config-if)# exit

PE1(config)# exit
PE1# write memory
```

##### Sur PE2 (R3) - Création des VRF

```cisco
PE2# configure terminal

! Créer VRF pour le Client A
PE2(config)# ip vrf CLIENT_A
PE2(config-vrf)# rd 100:1
PE2(config-vrf)# route-target export 100:1
PE2(config-vrf)# route-target import 100:1
PE2(config-vrf)# exit

! Créer VRF pour le Client B
PE2(config)# ip vrf CLIENT_B
PE2(config-vrf)# rd 100:2
PE2(config-vrf)# route-target export 100:2
PE2(config-vrf)# route-target import 100:2
PE2(config-vrf)# exit

! Associer les interfaces CE aux VRF
PE2(config)# interface FastEthernet1/0
PE2(config-if)# description Vers CE4 (R6) - Client B
PE2(config-if)# ip vrf forwarding CLIENT_B
PE2(config-if)# ip address 10.1.36.1 255.255.255.0
PE2(config-if)# no shutdown
PE2(config-if)# exit

PE2(config)# interface FastEthernet1/1
PE2(config-if)# description Vers CE2 (R7) - Client A
PE2(config-if)# ip vrf forwarding CLIENT_A
PE2(config-if)# ip address 10.1.37.1 255.255.255.0
PE2(config-if)# no shutdown
PE2(config-if)# exit

PE2(config)# exit
PE2# write memory
```

#### Vérification des VRF

```cisco
! Vérifier les VRF créées
PE1# show ip vrf

! Résultat attendu :
!   Name                             Default RD          Interfaces
!   CLIENT_A                         100:1               Fa1/0
!   CLIENT_B                         100:2               Fa1/1

! Vérifier les détails d'une VRF
PE1# show ip vrf detail CLIENT_A

! Vérifier la table de routage d'une VRF
PE1# show ip route vrf CLIENT_A
```

#### Étape 4 : Configuration des Routeurs CE

##### Configuration CE1 (R4) - Client A

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname CE1

! Configuration de l'interface vers PE1
CE1(config)# interface FastEthernet0/0
CE1(config-if)# description Vers PE1
CE1(config-if)# ip address 10.1.14.4 255.255.255.0
CE1(config-if)# no shutdown
CE1(config-if)# exit

! Configuration du réseau client
CE1(config)# interface FastEthernet0/1
CE1(config-if)# description Réseau Client A - Site 1
CE1(config-if)# ip address 172.16.4.1 255.255.255.0
CE1(config-if)# no shutdown
CE1(config-if)# exit

! Configuration d'une route par défaut vers le PE
CE1(config)# ip route 0.0.0.0 0.0.0.0 10.1.14.1
CE1(config)# exit

CE1# write memory
```

##### Configuration CE2 (R7) - Client A

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname CE2

! Configuration de l'interface vers PE2
CE2(config)# interface FastEthernet0/0
CE2(config-if)# description Vers PE2
CE2(config-if)# ip address 10.1.37.7 255.255.255.0
CE2(config-if)# no shutdown
CE2(config-if)# exit

! Configuration du réseau client
CE2(config)# interface FastEthernet0/1
CE2(config-if)# description Réseau Client A - Site 2
CE2(config-if)# ip address 172.16.7.1 255.255.255.0
CE2(config-if)# no shutdown
CE2(config-if)# exit

! Configuration d'une route par défaut vers le PE
CE2(config)# ip route 0.0.0.0 0.0.0.0 10.1.37.1
CE2(config)# exit

CE2# write memory
```

##### Configuration CE3 (R5) - Client B

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname CE3

! Configuration de l'interface vers PE1
CE3(config)# interface FastEthernet0/0
CE3(config-if)# description Vers PE1
CE3(config-if)# ip address 10.1.15.5 255.255.255.0
CE3(config-if)# no shutdown
CE3(config-if)# exit

! Configuration du réseau client
CE3(config)# interface FastEthernet0/1
CE3(config-if)# description Réseau Client B - Site 1
CE3(config-if)# ip address 172.16.5.1 255.255.255.0
CE3(config-if)# no shutdown
CE3(config-if)# exit

! Configuration d'une route par défaut vers le PE
CE3(config)# ip route 0.0.0.0 0.0.0.0 10.1.15.1
CE3(config)# exit

CE3# write memory
```

##### Configuration CE4 (R6) - Client B

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname CE4

! Configuration de l'interface vers PE2
CE4(config)# interface FastEthernet0/0
CE4(config-if)# description Vers PE2
CE4(config-if)# ip address 10.1.36.6 255.255.255.0
CE4(config-if)# no shutdown
CE4(config-if)# exit

! Configuration du réseau client
CE4(config)# interface FastEthernet0/1
CE4(config-if)# description Réseau Client B - Site 2
CE4(config-if)# ip address 172.16.6.1 255.255.255.0
CE4(config-if)# no shutdown
CE4(config-if)# exit

! Configuration d'une route par défaut vers le PE
CE4(config)# ip route 0.0.0.0 0.0.0.0 10.1.36.1
CE4(config)# exit

CE4# write memory
```

#### Étape 5 : Configuration MP-BGP entre PE

##### Sur PE1 (R1)

```cisco
PE1# configure terminal

! Configuration MP-BGP
PE1(config)# router bgp 100
PE1(config-router)# no bgp default ipv4-unicast
PE1(config-router)# bgp router-id 1.1.1.1

! Configuration du voisin iBGP avec PE2
PE1(config-router)# neighbor 3.3.3.3 remote-as 100
PE1(config-router)# neighbor 3.3.3.3 update-source Loopback0
PE1(config-router)# neighbor 3.3.3.3 next-hop-self

! Activation de VPNv4
PE1(config-router)# address-family vpnv4
PE1(config-router-af)# neighbor 3.3.3.3 activate
PE1(config-router-af)# neighbor 3.3.3.3 send-community extended
PE1(config-router-af)# exit-address-family

! Configuration pour CLIENT_A
PE1(config-router)# address-family ipv4 vrf CLIENT_A
PE1(config-router-af)# neighbor 10.1.14.4 remote-as 65001
PE1(config-router-af)# neighbor 10.1.14.4 activate
PE1(config-router-af)# network 172.16.4.0 mask 255.255.255.0
PE1(config-router-af)# exit-address-family

! Configuration pour CLIENT_B
PE1(config-router)# address-family ipv4 vrf CLIENT_B
PE1(config-router-af)# neighbor 10.1.15.5 remote-as 65002
PE1(config-router-af)# neighbor 10.1.15.5 activate
PE1(config-router-af)# network 172.16.5.0 mask 255.255.255.0
PE1(config-router-af)# exit-address-family

PE1(config-router)# exit
PE1(config)# exit
PE1# write memory
```

##### Sur PE2 (R3)

```cisco
PE2# configure terminal

! Configuration MP-BGP
PE2(config)# router bgp 100
PE2(config-router)# no bgp default ipv4-unicast
PE2(config-router)# bgp router-id 3.3.3.3

! Configuration du voisin iBGP avec PE1
PE2(config-router)# neighbor 1.1.1.1 remote-as 100
PE2(config-router)# neighbor 1.1.1.1 update-source Loopback0
PE2(config-router)# neighbor 1.1.1.1 next-hop-self

! Activation de VPNv4
PE2(config-router)# address-family vpnv4
PE2(config-router-af)# neighbor 1.1.1.1 activate
PE2(config-router-af)# neighbor 1.1.1.1 send-community extended
PE2(config-router-af)# exit-address-family

! Configuration pour CLIENT_A
PE2(config-router)# address-family ipv4 vrf CLIENT_A
PE2(config-router-af)# neighbor 10.1.37.7 remote-as 65001
PE2(config-router-af)# neighbor 10.1.37.7 activate
PE2(config-router-af)# network 172.16.7.0 mask 255.255.255.0
PE2(config-router-af)# exit-address-family

! Configuration pour CLIENT_B
PE2(config-router)# address-family ipv4 vrf CLIENT_B
PE2(config-router-af)# neighbor 10.1.36.6 remote-as 65002
PE2(config-router-af)# neighbor 10.1.36.6 activate
PE2(config-router-af)# network 172.16.6.0 mask 255.255.255.0
PE2(config-router-af)# exit-address-family

PE2(config-router)# exit
PE2(config)# exit
PE2# write memory
```

#### Étape 6 : Configuration BGP sur les CE

##### Sur CE1 (R4) et CE2 (R7) - Client A

```cisco
! Sur CE1
CE1(config)# router bgp 65001
CE1(config-router)# bgp router-id 4.4.4.4
CE1(config-router)# neighbor 10.1.14.1 remote-as 100
CE1(config-router)# network 172.16.4.0 mask 255.255.255.0
CE1(config-router)# exit

! Sur CE2
CE2(config)# router bgp 65001
CE2(config-router)# bgp router-id 7.7.7.7
CE2(config-router)# neighbor 10.1.37.1 remote-as 100
CE2(config-router)# network 172.16.7.0 mask 255.255.255.0
CE2(config-router)# exit
```

##### Sur CE3 (R5) et CE4 (R6) - Client B

```cisco
! Sur CE3
CE3(config)# router bgp 65002
CE3(config-router)# bgp router-id 5.5.5.5
CE3(config-router)# neighbor 10.1.15.1 remote-as 100
CE3(config-router)# network 172.16.5.0 mask 255.255.255.0
CE3(config-router)# exit

! Sur CE4
CE4(config)# router bgp 65002
CE4(config-router)# bgp router-id 6.6.6.6
CE4(config-router)# neighbor 10.1.36.1 remote-as 100
CE4(config-router)# network 172.16.6.0 mask 255.255.255.0
CE4(config-router)# exit
```

### 4. Vérification Complète MPLS VPN

#### Sur les Routeurs PE

```cisco
! Vérifier les routes VPNv4
PE1# show bgp vpnv4 unicast all

! Vérifier les voisins BGP
PE1# show ip bgp vpnv4 all summary

! Vérifier la table de routage pour chaque VRF
PE1# show ip route vrf CLIENT_A
PE1# show ip route vrf CLIENT_B

! Vérifier les détails d'une route VPN
PE1# show ip bgp vpnv4 vrf CLIENT_A 172.16.7.0

! Vérifier les labels MPLS
PE1# show mpls forwarding-table vrf CLIENT_A

! Vérifier les interfaces VRF
PE1# show ip vrf interfaces
```

#### Sur les Routeurs CE

```cisco
! Vérifier la table BGP
CE1# show ip bgp summary
CE1# show ip bgp

! Vérifier la table de routage
CE1# show ip route
```

### 5. Tests de Connectivité MPLS VPN

```cisco
! Test depuis CE1 (Client A - Site 1) vers CE2 (Client A - Site 2)
CE1# ping 172.16.7.1
CE1# traceroute 172.16.7.1

! Résultat attendu du traceroute :
! 1  10.1.14.1 [PE1]
! 2  10.1.12.2 [P] [MPLS]
! 3  10.1.23.2 [PE2]
! 4  10.1.37.7 [CE2]

! Test depuis CE3 (Client B - Site 1) vers CE4 (Client B - Site 2)
CE3# ping 172.16.6.1
CE3# traceroute 172.16.6.1

! Vérification de l'isolation : CE1 ne devrait PAS pouvoir pinger CE3
CE1# ping 172.16.5.1

! Résultat attendu : échec (les VPN sont isolés)
```

### 6. Tests Avancés et Vérifications

```cisco
! Sur PE, ping dans le contexte d'une VRF
PE1# ping vrf CLIENT_A 172.16.7.1
PE1# ping vrf CLIENT_B 172.16.6.1

! Vérifier le chemin MPLS complet
PE1# traceroute vrf CLIENT_A 172.16.7.1

! Debugger MPLS (utiliser avec précaution)
PE1# debug mpls packet
PE1# debug mpls ldp transport events

! Arrêter le debug
PE1# undebug all
```

### 7. Dépannage MPLS VPN

#### Problèmes Courants et Solutions

**1. Les voisins LDP ne s'établissent pas**

```cisco
! Vérifier OSPF dans le backbone
PE1# show ip ospf neighbor
PE1# show ip route

! Vérifier la configuration MPLS
PE1# show mpls interfaces
PE1# show mpls ldp discovery

! Vérifier la connectivité IP
PE1# ping 2.2.2.2 source 1.1.1.1
```

**2. Les routes VPN ne sont pas échangées**

```cisco
! Vérifier les voisins BGP
PE1# show ip bgp vpnv4 all summary

! Vérifier que VPNv4 est activé
PE1# show ip bgp neighbors 3.3.3.3

! Vérifier les RT (Route Targets)
PE1# show ip vrf detail
```

**3. Pas de connectivité entre sites**

```cisco
! Vérifier les routes dans la VRF
PE1# show ip route vrf CLIENT_A

! Vérifier que les routes sont apprises via BGP
PE1# show ip bgp vpnv4 vrf CLIENT_A

! Vérifier les labels
PE1# show mpls forwarding-table vrf CLIENT_A

! Tester depuis le PE
PE1# ping vrf CLIENT_A 172.16.7.1
```

**4. Réinitialisation des sessions**

```cisco
! Réinitialiser LDP
PE1# clear mpls ldp neighbor *

! Réinitialiser BGP
PE1# clear ip bgp * soft
```

### 8. Configuration Alternative avec Routes Statiques

Si vous ne souhaitez pas utiliser BGP entre PE et CE :

```cisco
! Sur PE1
PE1(config)# ip route vrf CLIENT_A 172.16.7.0 255.255.255.0 10.1.14.4

! Sur PE2
PE2(config)# ip route vrf CLIENT_A 172.16.4.0 255.255.255.0 10.1.37.7

! Redistribuer les routes statiques dans BGP
PE1(config)# router bgp 100
PE1(config-router)# address-family ipv4 vrf CLIENT_A
PE1(config-router-af)# redistribute static
PE1(config-router-af)# exit-address-family
```

---

## Conclusion

### Résumé des Apprentissages

Au cours de ce TP, vous avez appris à :

#### 1. Protocole OSPF

- **Comprendre** le fonctionnement d'un protocole à état de liens
- **Configurer** OSPF sur plusieurs routeurs avec différentes zones
- **Vérifier** les voisinages OSPF et les tables de routage
- **Analyser** la convergence du réseau et les chemins optimaux
- **Dépanner** les problèmes de routage OSPF courants

**Points clés :**

- OSPF utilise l'algorithme de Dijkstra pour calculer les meilleurs chemins
- La segmentation en aires améliore la scalabilité
- La convergence rapide garantit une haute disponibilité
- La configuration nécessite une planification minutieuse

#### 2. Protocole BGP

- **Maîtriser** les concepts de systèmes autonomes (AS)
- **Configurer** des sessions eBGP entre différents AS
- **Comprendre** les attributs BGP et les politiques de routage
- **Implémenter** l'agrégation et le filtrage de routes
- **Analyser** les décisions de routage BGP

**Points clés :**

- BGP est le protocole de routage d'Internet
- Les politiques de routage offrent un contrôle précis
- La configuration manuelle des sessions garantit la sécurité
- Les attributs BGP influencent la sélection des routes

#### 3. Protocole MPLS VPN

- **Comprendre** l'architecture MPLS avec les routeurs CE, P et PE
- **Configurer** des VRF pour isoler les trafics clients
- **Implémenter** MP-BGP pour l'échange de routes VPN
- **Activer** le label switching avec LDP
- **Vérifier** l'isolation entre différents VPN
- **Tester** la connectivité de bout en bout

**Points clés :**

- MPLS améliore l'efficacité du routage par commutation d'étiquettes
- Les VRF assurent l'isolation complète entre clients
- MP-BGP avec VPNv4 permet l'échange de routes entre PE
- L'architecture à trois niveaux (CE-PE-P) optimise les performances

### Compétences Acquises

À l'issue de ce TP, vous êtes capable de :

1. **Concevoir** des architectures réseau complexes utilisant plusieurs protocoles de routage
2. **Implémenter** des solutions de routage adaptées à différents contextes
3. **Configurer** des équipements réseau Cisco pour OSPF, BGP et MPLS
4. **Vérifier** et **valider** le bon fonctionnement des protocoles
5. **Diagnostiquer** et **résoudre** les problèmes de routage
6. **Analyser** les flux de trafic dans des réseaux multi-protocoles
7. **Sécuriser** et **isoler** le trafic avec des VPN MPLS

### Applications Pratiques

Ces technologies sont utilisées dans :

- **Réseaux d'entreprise** : OSPF pour le routage interne
- **Fournisseurs d'accès Internet** : BGP pour l'interconnexion
- **Opérateurs télécoms** : MPLS VPN pour les services aux entreprises
- **Data centers** : Combinaison de protocoles pour l'optimisation
- **Cloud computing** : Interconnexion de sites distants

### Pour Aller Plus Loin

#### Sujets Avancés à Explorer

1. **OSPF Avancé**
   - Configuration multi-aires complexe
   - Stub areas et NSSA
   - Authentification OSPF
   - Optimisation des LSA

2. **BGP Avancé**
   - Communities BGP
   - AS-Path prepending
   - Local Preference et MED
   - BGP Route Reflectors

3. **MPLS Avancé**
   - Traffic Engineering (MPLS-TE)
   - Fast Reroute (FRR)
   - QoS dans MPLS
   - MPLS pour IPv6 (6PE/6VPE)

4. **Sécurité**
   - Authentication des protocoles
   - Filtrage et contrôle d'accès
   - BGP Security (RPKI, BGPsec)
   - VPN IPsec sur MPLS

### Ressources Complémentaires

- **Documentation Cisco** : cisco.com/go/docs
- **RFC officielles** : OSPF (RFC 2328), BGP (RFC 4271), MPLS (RFC 3031)
- **Laboratoires en ligne** : Cisco Packet Tracer, EVE-NG, GNS3
- **Certifications** : CCNA, CCNP Enterprise, CCNP Service Provider

---

## Annexes

### Annexe A : Commandes de Vérification Rapide

```cisco
! OSPF
show ip ospf neighbor
show ip ospf interface brief
show ip route ospf
show ip ospf database

! BGP
show ip bgp summary
show ip bgp
show ip bgp neighbors
show ip route bgp

! MPLS
show mpls ldp neighbor
show mpls forwarding-table
show mpls interfaces
show ip vrf
show ip route vrf [nom-vrf]
show bgp vpnv4 unicast all

! Général
show ip interface brief
show running-config
show version
ping [destination]
traceroute [destination]
```

### Annexe B : Plan d'Adressage Suggéré

#### Backbone MPLS

- PE1 Loopback : 1.1.1.1/32
- P Loopback : 2.2.2.2/32
- PE2 Loopback : 3.3.3.3/32
- PE1-P : 10.1.12.0/24
- P-PE2 : 10.1.23.0/24

#### Client A (VPN A)

- Site 1 : 172.16.4.0/24
- Site 2 : 172.16.7.0/24
- PE1-CE1 : 10.1.14.0/24
- PE2-CE2 : 10.1.37.0/24

#### Client B (VPN B)

- Site 1 : 172.16.5.0/24
- Site 2 : 172.16.6.0/24
- PE1-CE3 : 10.1.15.0/24
- PE2-CE4 : 10.1.36.0/24

