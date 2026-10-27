
#Subnetting #adressage #IPv6
# Module 12 Adressage IPv6

## 1 Problèmes liés à l'IPv4 & Nécessité d'IPv6

### 1.1. Limites d'IPv4
* **Taille d'adresse :** 32 bits (~4,3 milliards d'IPs) $\implies$ Pénurie mondiale actée par les RIR.
* **Inconvénients du NAT :** Solution temporaire qui brise le modèle bout-en-bout et nuit aux applications temps réel/P2P.

### 1.2. Apports de l'IPv6
* **Taille d'adresse :** **128 bits** ($2^{128} \approx 340 \text{ undécillions}$).
* **Protocole ICMPv6 :** Intègre l'autoconfiguration (SLAAC) et la résolution d'adresses (remplace ARP).
* IPv6 pas que remplacé IPv4 aussi l'améliorer
voir [[Module 08 Couche reseau#6.4. Champs de l'en-tête de paquet IPv6]]

### 1.3. Les 3 Techniques de Transition


> [!KEY]- À retenir
> 1. **Double pile (Dual-Stack) :** IPv4 et IPv6 fonctionnent en parallèle sur les mêmes équipements. **(Méthode recommandée)**.
> 2. **Tunneling :** Encapsulation d'un paquet IPv6 dans un paquet IPv4 pour traverser une infrastructure IPv4.
> 3. **Traduction (NAT64) :** Conversion à la volée des paquets IPv6 $\leftrightarrow$ IPv4 pour l'interconnexion de réseaux incompatibles.



> [!note]-  
> * **Double pile (Dual-Stack) :** IPv4 et IPv6 fonctionnent en parallèle sur les mêmes équipements. **(Méthode recommandée)**.
>  * **Tunneling :** Encapsulation d'un paquet IPv6 dans un paquet IPv4 pour traverser une infrastructure IPv4.
> * **Traduction (NAT64) :** Conversion à la volée des paquets IPv6 $\leftrightarrow$ IPv4 pour l'interconnexion de réseaux incompatibles.
>>>>>>> b0d41ef71c10ec546bac96cd8f8b6719b15a4add
## 2 Représentation de l'adresse IPv6

### 2.1. Format de base
* **Longueur :** 
	* **128 bits** codés en hexadécimal (non sensible à la casse).
* **Structure :** 
	* 8 blocs de 16 bits appelés **hextets** (séparés par des deux-points `:`).
* **Composition :** 
	* 1 hextet = 4 caractères hexadécimaux = 16 bits binaires.
	* Exemple complet :* `2001:0db8:0000:1111:0000:0000:0000:0200`


### 2.2. Les 2 Règles de Compression (Abréviation)

> [!KEY]- Règle 1 : Omission des zéros en TÊTE de segment
> * On supprime uniquement les `0` situés **au début** d'un hextet.
> * Les zéros de fin **ne doivent jamais** être supprimés (créerait de l'ambiguïté).
> 
> **Exemples :**
> * `01AB` $\to$ `1AB` | `09f0` $\to$ `9f0` | `000A` $\to$ `A` | `0000` $\to$ `0`
> * `2001:0db8:0000:00a3:...` $\to$ `2001:db8:0:a3:...`



> [!KEY]- Règle 2 : Le double deux-points (`::`)
> * Remplace une chaîne contiguë d'un ou plusieurs hextets entièrement nuls (`0000`).
> * **Règle absolue :** Utilisable **UNE SEULE FOIS** par adresse (sinon l'adresse devient ambiguë/impossible à reconstituer).
> * *Meilleure pratique :* Appliquer `::` sur la plus longue séquence de zéro contiguë.
> 
> **Exemples d'application combinée (Règle 1 + Règle 2) :**
> * `2001:0db8:0000:1111:0000:0000:0000:0200` $\to$ `2001:db8:0:1111::200`
> * `fe80:0000:0000:0000:0000:0000:0000:0001` $\to$ `fe80::1`
> * `0000:0000:0000:0000:0000:0000:0000:0000` $\to$ `::`


## 3. Types d'adresses IPv6

### 3.1. Catégories d'adresses IPv6 
* **Monodiffusion (Unicast) :** 
	* Identifie une interface unique de façon exclusive. Tout paquet IPv6 envoyé doit avoir une adresse source Unicast.
* **Multidiffusion (Multicast) :** 
	* Utilisée pour envoyer un seul paquet IPv6 vers plusieurs destinations simultanément.
* 🚨🆕**Anycast :** 
	* Adresse Unicast attribuée à plusieurs équipements ; le paquet est acheminé vers le périphérique le plus proche supportant cette adresse.
* 🚨🆕**Absence de Broadcast :** 
	* IPv6 n'a **pas d'adresse de diffusion (broadcast)**. Il utilise la multidiffusion (notamment l'adresse multicast « tous les nœuds ») pour obtenir le même résultat.

### 3.2. Longueur de préfixe & Structure 
* **Rôle :** 
	* Indique la partie réseau de l'adresse IPv6 (remplace la notion de masque de sous-réseau décimal IPv4).
* **Plage possible :** 
	* La longueur du préfixe peut être comprise **entre 0 et 128**.
* **Préfixe recommandé :** 
	* **/64** pour les réseaux locaux (LAN) et la plupart des sous-réseaux.
  * **Structure /64 :** 
	  * 64 bits de préfixe (partie réseau) + 64 bits d'ID d'interface (partie hôte).
  * **Raison :** 
	  * Un ID d'interface de 64 bits est obligatoire pour faire fonctionner l'autoconfiguration sans état (**SLAAC**) et simplifie la création/gestion des sous-réseaux.


<<<<<<< HEAD
> [!NOTE]- SLAAC (*Stateless Address Autoconfiguration*)
=======
> [!NOTE]-  **SLAAC** (*Stateless Address Autoconfiguration*)
>>>>>>> b0d41ef71c10ec546bac96cd8f8b6719b15a4add
> **SLAAC** permet à un équipement de se brancher sur un réseau IPv6 et d'obtenir immédiatement une IP globale fonctionnelle **sans aucune intervention d'un serveur DHCPv6**. 
> * **Stateless (Sans état) :** Le routeur ne conserve aucune trace (aucun état) des adresses distribuées aux équipements.
> 
> * **Pourquoi un /64 obligatoire ?** Le mécanisme SLAAC prend le préfixe de 64 bits fourni par le routeur local et y colle un identifiant d'interface de 64 bits généré par l'appareil lui-même.
> * **Avantage clé :** Simplification totale du déploiement (autoconfiguration Plug-and-Play) tout en conservant une passerelle par défaut opérationnelle (la LLA du routeur).


### 3.3. Types d'adresses Unicast fondamentales 

Un équipement IPv6 possède généralement **au moins deux adresses Unicast** sur la même interface : une GUA et une LLA.

| Type d'adresse Unicast             | Plage / Préfixe                     | Description & Caractéristiques clés                                                              |
| :--------------------------------- | :---------------------------------- | :----------------------------------------------------------------------------------------------- |
| **GUA** *(Global Unicast Address)* | `2000::/3` (`2000...` à `3FFF...`)  | Public & Routable mondialement sur Internet (équivalent IP publique IPv4).                       |
| **LLA** *(Link-Local Address)*     | `fe80::/10` (`fe80...` à `febf...`) | Obligatoire sur chaque interface IPv6. Communication limitée à la liaison locale (non routable). |
| **ULA** *(Unique Local Address)*   | `fc00::/7` (`fc00...` à `fdff...`)  | Utilisation locale au site/entreprise (équivalent IP privée ). Non routée sur l'Internet public. |
| **Bouclage (Loopback)**            | `::1/128`                           | Adresse de test local de la pile réseau (équivalent de `127.0.0.1` en IPv4).                     |
| **Non spécifiée**                  | `::/128`                            | Adresse temporaire tout-zéro utilisée lorsqu'aucune IP n'est encore attribuée à l'interface.     |

> [!WARNING]- Remarque importante : Adresse Locale Unique - ULA (`fc00::/7` à `fdff::/7`)
> 
> * **Statut d'implémentation :** Les ULA ne sont **pas encore couramment implémentées** dans la pratique. Le cours NetAcad se concentre donc quasi exclusivement sur la configuration des adresses **GUA** et
> * Quel intérêt ?


### 3.4. Structure d'une adresse GUA 

Structure classique d'une adresse **GUA /64** distribuée à partir d'un bloc entreprise **/48** :

```text
|<------------ 64 bits ------------>|<------- 64 bits ------->|
+-------------------+---------------+-------------------------+
| Global Routing    | Subnet ID     | Interface ID            |
| Prefix (/48)      | (16 bits)     | (64 bits)               |
+-------------------+---------------+-------------------------+
|<--- 3 hextets --->|<- 1 hextet -->|<------ 4 hextets ------>|
```

- **Préfixe de routage global (48 bits en général) :** 
	- Attribué par le FAI ou l'IANA/RIR. Les 3 premiers bits d'une GUA sont toujours `001` (d'où le premier chiffre hexa compris entre `2` et `3`).
    
- **ID de sous-réseau (16 bits) :** 
	- Champ situé entre le préfixe global et l'ID d'interface. Permet à une organisation de créer jusqu'à **65 536 sous-réseaux /64** sans toucher aux bits d'hôte.
    
- **ID d'interface (64 bits) :** 
	- Équivalent de la partie hôte IPv4. Permet d'accueillir jusqu'à 18 quintillions d'hôtes par sous-réseau.
    
- **Spécificité IPv6 :** 
	- Les adresses réseau tout-`0` et tout-`1` (utilisables pour les hôtes) ne sont pas perdues comme en IPv4, bien que l'adresse tout-`0` soit réservée comme adresse Anycast du routeur sous-réseau.
    

### 3.5. Adresse LLA - Link-Local Address

- **Portée stricte :** 
	- Utilisée uniquement pour la communication entre équipements sur le **même segment local**. Les routeurs ne transfèrent jamais un paquet dont l'IP source ou destination est une LLA.
    
- **Rôle crucial :**
    
    - Sert de **passerelle par défaut (Default Gateway)** pour les hôtes du réseau.
        
    - Sert pour l'échange de messages des protocoles de routage (OSPFv3, RIPng...).
        
- **Plage binaire :** 
	- Le 1er hextet est compris entre `fe80` et `febf` (10 bits fixés à `1111 1110 10xx xxxx`).
    
- **Modes de configuration :**
    
    - **Statique :** Définie manuellement par l'administrateur (ex: `fe80::1` pour un routeur).
        
    - **Dynamique :** Générée automatiquement par l'appareil à l'activation de l'interface (via méthode aléatoire ou **EUI-64** basée sur la MAC address).

> [!NOTE]- EUI-64 en bref
> **EUI-64** (*Extended Unique Identifier 64 bits*) convertit une adresse **MAC (48 bits)** en un **ID d'interface IPv6 (64 bits)** en 2 actions :
> 1. **Insertion de `FF-FE`** au milieu de l'adresse MAC.
> 2. **Inversion du 7ᵉ bit** du premier octet (le bit *Universel/Local*).
> 
> * **Repérage rapide :** Si une IP contient `ff:fe` au milieu de son ID d'interface, elle provient d'EUI-64 (ex: `fe80::fc99:47`**`ff:fe`**`75:cee0`).
> * **Problème :** Expose la carte MAC (traçabilité). Les PC préfèrent aujourd'hui la génération aléatoire.





## 4 Configuration statique de GUA et LLA

### 4.1. Synthèse des commandes Cisco IOS

* La syntaxe est identique à l'IPv4 en remplaçant `ip` par `ipv6`.
* **GUA :** La longueur de préfixe est collée à l'adresse sans espace (`/64`).
* **LLA :** Utilise le mot-clé obligatoire `link-local` à la fin de la commande.
* Ne PAS OUBLIER  le fameux `no shutdown` 

```IOS
! Configuration GUA & LLA sur routeur (R1)
interface gigabitethernet 0/0/0
 ipv6 address 2001:db8:acad:1::1/64
 ipv6 address fe80::1:1 link-local
 no shutdown
```

### 4.2. Configuration GUA 

- **Routeur :** Une interface peut héberger simultanément une GUA et une LLA.
    
- **Hôte (PC) :**    
    - Configuration manuelle possible (IP, préfixe, passerelle par défaut).
        
    - **Meilleure pratique Passerelle :** 
	    - Utiliser l'adresse **LLA du routeur** comme passerelle par défaut au lieu de sa GUA.
        
    - **Attribution dynamique :** 
	    - Préférée en entreprise via **SLAAC** ou **DHCPv6** (définit automatiquement la LLA du routeur comme passerelle).
        

### 4.3. Configuration LLA 

- **Intérêt :** 
	- Définir des adresses simples et mémorisables pour les passerelles et le routage (ex: `fe80::1` pour R1, `fe80::2` pour R2).
    
- **Portée & Réutilisation :**
    
    - Une même LLA peut être configurée sur plusieurs interfaces d'un même routeur, à condition d'être unique sur chaque lien local.
        
    - **Bonne pratique :** Varier légèrement l'adresse par interface (ex: `fe80::1:1`, `fe80::2:1`) pour faciliter l'identification.


## 5 Adressage Dynamique des GUA IPv6

### 5.1. Préréquis Routeur & Messages ICMPv6 

* **Activation du routage IPv6 :** Obligatoire sur le routeur avec la commande globale `ipv6 unicast-routing` (sinon il n'envoie pas de messages RA).
* **Messages de dialogue ICMPv6 :**
  * **RS (Router Solicitation) :** Envoyé par l'hôte à l'allumage pour demander des informations d'adressage.
  * **RA (Router Advertisement) :** Envoyé par le routeur (périodiquement toutes les 200s ou en réponse à un RS) pour fournir le préfixe réseau, la longueur de préfixe (`/64`), la passerelle par défaut (adresse LLA du routeur) et les options DNS.



### 5.2. Les 3 Méthodes d'attribution dynamique 

Le message RA indique à l'hôte la méthode à suivre via des indicateurs (flags) :

| Méthode       | Nom                             | Fonctionnement                                                                          | Ce que fournit le Routeur (RA)          | Ce que fournit le Serveur DHCPv6   |
| :------------ | :------------------------------ | :-------------------------------------------------------------------------------------- | :-------------------------------------- | :--------------------------------- |
| **Méthode 1** | **SLAAC uniquement**            | L'hôte crée sa GUA en autonomie complète sans serveur DHCPv6.                           | Préfixe, Masque `/64`, Passerelle (LLA) | *Aucun serveur utilisé*            |
| **Méthode 2** | **SLAAC + DHCPv6 sans état**    | L'hôte génère son IP via SLAAC et sollicite DHCPv6 pour les options annexes.            | Préfixe, Masque `/64`, Passerelle (LLA) | Serveurs DNS, Nom de domaine       |
| **Méthode 3** | **Stateful DHCPv6 (avec état)** | Fonctionne comme le DHCP IPv4 classique. Le serveur attribue et conserve l'état des IP. | Passerelle par défaut (LLA du routeur)  | Adresse GUA complète, DNS, options |

> [!NOTE]- Passerelle par défaut
> Quelle que soit la méthode (1, 2 ou 3), l'adresse de la **passerelle par défaut est TOUJOURS la LLA du routeur** transmise via le message RA. Un serveur DHCPv6 ne fournit jamais la passerelle par défaut.



### 5.3. Génération de l'ID d'interface (64 bits) 

Lorsque SLAAC est utilisé (Méthodes 1 et 2), le client doit fabriquer lui-même son ID d'interface (64 bits) selon deux méthodes :

#### A. Méthode EUI-64 (Basée sur l'adresse MAC 48 bits) 

1. Diviser l'adresse MAC Ethernet (48 bits) en deux blocs de 24 bits (OUI et ID périphérique).
2. Insérer la valeur hexadécimale **`FF-FE`** (16 bits) au milieu.
3. Inverser le 7ᵉ bit (bit U/L) du premier octet (ex: `0` devient `1`).
* *Inconvénient :* Traçabilité et risque pour la confidentialité (l'adresse MAC est exposée dans l'IP). Repérable dans un `ipconfig` par la présence de `ff:fe`.

> [!tip]- Exemple pratique
> Soit une adresse MAC : `FC-99-47-75-CE-E0`
> * **Étape 1 (Division) :** `FC-99-47`  |  `75-CE-E0`
> * **Étape 2 (Insertion FF-FE) :** `FC-99-47`-**`FF-FE`**-`75-CE-E0`
> * **Étape 3 (Inversion bit 7) :**
> * `FC` en binaire = `1111 1100`
> * Inversion du 7ᵉ bit $\to$ `1111 1110` = `FE` en hexadécimal
> * **Résultat EUI-64 final :** `FE99:47FF:FE75:CEE0`

#### B. Génération aléatoire (Random)
* Un nombre de 64 bits généré aléatoirement par le système d'exploitation (standard par défaut depuis Windows Vista/Linux/macOS pour protéger la vie privée).

### 5.4. Vérification d'unicité : DAD (Duplicate Address Detection)
* Avant d'utiliser toute adresse IPv6 (GUA ou LLA) générée dynamiquement, l'hôte exécute l'algorithme **DAD** (requête ICMPv6 voisine) pour vérifier qu'aucun autre équipement local n'utilise la même IP.

## 6 LLA dynamiques et Vérification IPv6

### 6.1. Génération dynamique des LLA 

Toute interface compatible IPv6 génère automatiquement une adresse **LLA** (`fe80::/10`) dès son activation.

* **Mécanisme :** 
	* L'adresse est créée en combinant le préfixe `fe80::/10` avec un ID d'interface de 64 bits généré par **EUI-64** ou de manière **aléatoire**.
* **Comportement des clients (Windows) :** 
	* Utilise la même méthode d'ID d'interface (EUI-64 ou aléatoire) pour sa LLA et pour sa GUA (via SLAAC).
* **Comportement des routeurs Cisco IOS :**
	* Génèrent leur LLA dynamique par défaut via la méthode **EUI-64** basée sur la MAC address de la première interface Ethernet disponible.
	* Pour les interfaces série (sans MAC), le routeur réutilise la MAC d'une interface Ethernet.
	* *Inconvénient :** Les LLA générées dynamiquement sont longues et complexes à mémoriser (ex: `fe80::7279:b3ff:fe92:3640`). C'est pourquoi la **configuration statique** (ex: `fe80::1`) est la meilleure pratique sur les routeurs.

---

### 6.2. Commandes de vérification de la configuration IPv6 

| Commande Cisco IOS          | Description & Points clés à retenir                                                                                                                                                                            |
| :-------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `show ipv6 interface brief` | Affiche l'état des interfaces (`up/up`) ainsi que leurs adresses **LLA et GUA**. *(Note : Les LLA n'apparaissent pas dans la table de routage).*                                                               |
| `show ipv6 route`           | Affiche la table de routage IPv6 :<br>• **`C` (Directly Connected) :** Réseau IPv6 connecté (`/64`).<br>• **`L` (Local) :** Route locale vers l'adresse exacte de l'interface (`/128`).                        |
| `ping <adresse_IPv6>`       | Teste la connectivité ICMPv6.<br>• *Règle LLA :* En cas de ping vers une adresse **LLA**, Cisco IOS demande de préciser **l'interface de sortie**, car une même LLA peut exister sur plusieurs liens distants. |

## 7 Adresses de multidiffusion (Multicast) IPv6

### 7.1. Principes généraux 

* **Préfixe réservé :** 
	* **`ff00::/8`** (toutes les adresses multicast commencent par `ff...`).
* **Règle stricte :** 
	* Une adresse de multidiffusion ne peut être qu'une **adresse de destination** (jamais une adresse source). ce n'est pas NOUVEAU !
* **Deux types principaux :**
  1. Les adresses de multidiffusion attribuées.
  2. Les adresses de multidiffusion de nœud sollicité (*Solicited-Node Multicast*).


### 7.2. Adresses de multidiffusion attribuées courantes

Réseaux/groupes pré-définis auxquels les équipements s'abonnent automatiquement :

| Adresse Multicast | Groupe de destination | Description & Utilisation |
| :--- | :--- | :--- |
| **`ff02::1`** | **Tous les nœuds** (*All-Nodes*) | Remplace le broadcast IPv4[cite: 42]. Tous les équipements IPv6 du lien reçoivent et traitent le paquet[cite: 42]. *(Ex : Envoi des messages RA par le routeur)*[cite: 42]. |
| **`ff02::2`** | **Tous les routeurs** (*All-Routers*) | Rejoint par tous les routeurs IPv6 du lien (activés via `ipv6 unicast-routing`)[cite: 42]. *(Ex : Envoi des messages RS par les hôtes)*[cite: 42]. |


### 7.3. Multidiffusion de nœud sollicité (*Solicited-Node*) 

* **Définition :** 
	* Adresse créée automatiquement et associée à chaque adresse Unicast (GUA ou LLA) attribuée à une interface.
* **Fonctionnement matériel :** 
	* Mappée directement sur une adresse MAC multicast Ethernet spécifique.
* **Avantage clé :** 
	* Permet à la carte réseau Ethernet d'un hôte de **filtrer la trame au niveau matériel (Couche 2)** sans solliciter le processeur/pile IPv6 si la trame ne lui est pas destinée.
* **Rôle principal :** 
	* Utilisée à la place du broadcast ARP pour la résolution d'adresses MAC et la détection d'adresses dupliquées (**DAD**).

## 8. Subnetting IPv6

### 8.1. Principe de conception 
* **Intégration native :** 
	* IPv6 a été conçu dès l'origine pour faciliter le Subnetting grâce au champ dédié **Subnet ID**.
* **Structure d'un bloc /48 attribué :**
	* *Global Routing Prefix :** 48 bits (fournis par l'ISP/IANA).
	* *Subnet ID :** **16 bits** (4ᵉ hextet).
	* *Interface ID :** **64 bits** (standard obligatoire pour SLAAC).
* **Capacité :** 
	* Un champ Subnet ID de 16 bits offre **65 536 sous-réseaux /64**, chacun pouvant accueillir 18 quintillions d'hôtes.


### 8.2. Mécanique d'Incrémentation Hexadécimale 
Contrairement à l'IPv4, **aucune conversion binaire n'est nécessaire**. Pour créer les sous-réseaux successifs, il suffit d'incrémenter le 4ᵉ hextet en **hexadécimal** :

* Bloc racine : `2001:db8:acad::/48`
	* Sous-réseau 0 : `2001:db8:acad:0000::/64` (ou `2001:db8:acad::/64`)
	* Sous-réseau 1 : `2001:db8:acad:0001::/64`
	* Sous-réseau 2 : `2001:db8:acad:0002::/64`
	* ...
	* Sous-réseau 10 : `2001:db8:acad:000a::/64`
	* ...
	* Dernier sous-réseau : `2001:db8:acad:ffff::/64`

> [!NOTE] Pas de contrainte de gaspillage sur les liaisons Point-à-Point
> Sur les liaisons série ou WAN (routeur à routeur), on utilise également un masque **/64** standard. Même si 2 adresses IP seulement sont consommées sur les quintillions disponibles, conserver le préfixe `/64` préserve l'homogénéité du plan d'adressage et la compatibilité globale.