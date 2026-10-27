# Couche Liaison de données (OSI layer 2)

La couche **liaison de données** (Couche 2) : 
- prépare les données du réseau pour le support physique 
- et assure la communication directe  NIC à NIC(Network Interface card). 

Dans un réseau informatique, un **nœud** est un équipement/périphérique connecté capable de **recevoir, créer, stocker ou transférer des données** le long d'un chemin de communication.
Il existe deux catégories de nœuds :
- **Périphériques finaux (hôtes) :** Équipements source ou destination (ex: ordinateurs portables, smartphones, serveurs)
    
- **Périphériques intermédiaires :** Équipements assurant le transit des données (ex: commutateurs Ethernet, routeurs).

Différents types de supports pour différents types de données 

- Rôles et fonctions clés :

	- **Abstraction du support :** Permet aux couches supérieures (3 et +) d'accéder au support physique sans connaître la technologie matérielle utilisée.
    
	- **Encapsulation :** Reçoit les paquets de Couche 3 (IPv4/IPv6) et les encapsule dans des **trames** de Couche 2.
    
	- **Contrôle d'accès au support (MAC) :** Régit la façon dont les trames sont placées et reçues sur le média réseau.
    
	- **Échange de trames :** Achemine les trames entre les équipements locaux via le support.
    
	- **Décapsulation :** Extrait le paquet de Couche 3 d'une trame reçue et le transmet au protocole supérieur approprié.
    
	- **Détection d'erreurs :** Identifie et rejette automatiquement toutes les trames altérées ou corrompues pendant le transit.

![[Pasted image 20260930145840.png]]

## Sous-couches liaison de données IEEE 802 (LAN/MAN)

La couche 2 se divise en deux sous-couches sous les normes IEEE 802 :

- **LLC (Logical Link Control - IEEE 802.2) :**
    
    - Fait l'intermédiaire entre  couche supérieur (L3)  et le matériel (L1/L2).
        
    - Identifie le protocole de couche 3 (ex: IPv4, IPv6) pour permettre le partage du même support.
        
- **MAC (Media Access Control - IEEE 802.3, 802.11, 802.15) :**
    
    - Implémentée dans le matériel (carte réseau).
        
    - Gère l'encapsulation des données, l'adressage physique (adresse MAC) et le contrôle d'accès au support.
![[Pasted image 20260930150422.png]]

### Structure de la couche Liaison de données (IEEE 802)

| **Sous-couche**                  | **Norme**                                                                                                            | **Rôle principal**                                                                                                   |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **LLC** (_Logical Link Control_) | **IEEE 802.2**                                                                                                       | Ajoute les informations de contrôle de Couche 2 au paquet L3 (IPv4/IPv6) pour l'acheminer vers le nœud destinataire. |
| **MAC** (_Media Access Control_) | **IEEE 802.3** (Ethernet)<br><br>  <br><br>**IEEE 802.11** (Wi-Fi)<br><br>  <br><br>**IEEE 802.15** (WPAN/Bluetooth) | Pilote directement la carte réseau (NIC) et le matériel de transmission.                                             |

### Fonctions clés de la sous-couche MAC

1. **Encapsulation des données :**
    
    - **Délimitation des trames :** Insère des bits de synchronisation entre l'émetteur et le récepteur pour marquer le début/fin de trame.
        
    - **Adressage :** Ajoute les adresses physiques MAC source et destination pour le transport local.
        
    - **Détection d'erreurs :** Intègre une séquence de contrôle de trame (queue de bande/FCS) pour valider l'intégrité de la transmission.
        
2. **Contrôle d'accès au support :**
    
    - Régit la communication entre plusieurs équipements sur un support partagé (Half-Duplex).
        
    - Note : Inutile en Full-Duplex (liens dédiés).
### Accès aux supports

Chaque segment de réseau peut utiliser un support et des règles d'accès différents (ex: Ethernet multi-accès vs liaison série point-à-point). La sous-couche MAC adapte la trame à chaque type de support.


À chaque passage d'un réseau à un autre, le routeur effectue les **4 étapes de Couche 2** suivantes :

1. **Réception :** Accepte la trame provenant du support d'entrée.
    
2. **Désencapsulation :** Retire l'en-tête et la queue de bande L2 pour extraire le paquet L3 (IP).
    
3. **Réencapsulation :** Encapsule le paquet L3 dans une **nouvelle trame** adaptée au support de sortie.
    
4. **Transmission :** Expédie la nouvelle trame sur le segment physique suivant.

### Normes de couche liaison de données

Contrairement aux couches supérieures TCP/IP définies par les RFC de l'**IETF**, la couche liaison de données (Accès réseau) dépend d'organismes d'ingénierie matérielle.

#### Principaux organismes de normalisation (L1 / L2) :

- **IEEE** (_Institute of Electrical and Electronics Engineers_) : Dfinit les normes LAN/WLAN (802.3, 802.11).
    
- **UIT / ITU** (_Union Internationale des Télécommunications_) : Normes de télécommunications.
    
- **ISO** (_International Standards Organization_) : Normalisation internationale des réseaux.
    
- **ANSI** (_American National Standards Institute_) : Normes d'interfaces et de codage.

### Topologies et contrôle d'accès au support

#### 1. Topologies WAN 

- **Point à point :** Liaison directe et permanente entre deux nœuds.
    
    - Protocoles L2 simples ex: PPP(_Point-to-Point Protocol_).
        
    - La présence d'équipements intermédiaires au sein du réseau d'opérateur ne modifie pas la topologie logique point à point.
        
- **Hub et Spoke :** Site central (_Hub_) connecté en point à point à plusieurs sites distants (_Spokes_).
    
- **Maillée :** Haute disponibilité ; chaque site est connecté à tous les autres (maillage total) ou à certains sites (maillage partiel).
    

#### 2. Topologies LAN

- **Modernes :**
    
    - **Étoile :** Hôtes reliés à un switch central.
        
    - **Étoile étendue :** Interconnexion de plusieurs switch en étoile.
        
- **Anciennes :**
    
    - **Bus :** Câble coaxial partagé avec bouchons de terminaison aux extrémités.
        
    - **Anneau :** Interconnexion circulaire (ex: Token Ring, FDDI).
        

#### 3. Modes Duplex 

- **Semi-duplex (Half-Duplex) :** Transmission ou réception alternée (une seule direction à la fois).
    
    - Utilisé sur les anciens hubs, le Wi-Fi et les topologies en bus.
        
- **Duplex intégral (Full-Duplex) :** Transmission et réception simultanées.
    
    - Mode standard des switch Ethernet modernes (aucun risque de collision).
        

#### 4. Méthodes de contrôle d'accès 
dans les réseau d'accès multiple (LAN/WLAN) accès simultané au support physique de 2 périphériques 

> [!Note] 
>  **CSMA** (_Carrier Sense Multiple Access_ / Accès multiple avec écoute de porteuse) est un protocole de **Couche 2** utilisé sur les réseaux à support partagé en **semi-duplex** pour contrôler l'accès aux médias physiques.


- **Accès avec gestion des conflits (Non déterministe) :**
    
    - **CSMA/CD :** Détection de collision (anciens LAN Ethernet en bus/hub/half-duplex).
        
    - **CSMA/CA :** Évitement de collision (réseaux sans fil WLAN / Wi-Fi).
        
	
| **Critère**                | **CSMA/CD (Collision Detection)**                                                          | **CSMA/CA (Collision Avoidance)**                                                       |
| -------------------------- | ------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------- |
| **Environnement**          | Ethernet filaire ancien (Hubs, bus)                                                        | Réseaux sans fil (Wi-Fi / WLAN)                                                         |
| **Mode Duplex**            | Semi-duplex (_Half-Duplex_)                                                                | Semi-duplex (_Half-Duplex_)                                                             |
| **Gestion des collisions** | **Réagit** aux collisions une fois qu'elles se produisent.                                 | **Évite** préventivement les collisions.                                                |
| **Mécanisme**              | Écoute → Émission → Détection par hausse d'amplitude → Signal Jamming + Attente aléatoire. | Écoute → Attente (IFS) → Émission d'un signal d'intention (RTS/CTS) → Validation (ACK). |
| **Transmission**           | Envoie la trame directement si le canal est libre.                                         | Annonce la durée de transmission et attend une confirmation pour émettre.               |
	
- **Accès contrôlé (Déterministe) :**
    
    - Passage de jeton à tour de rôle (ex: Token Ring, ARCNET).



> [!remarque] 
>  Aujourd'hui, les réseau Ethernet fonctionne en duplex intégral et ne nécessite pas de méthode d'accès.

### Structure de la trame et adressage L2

#### 1. Structure de la trame

La couche liaison de données encapsule les paquets L3 dans des trames composées de 3 éléments principaux :

![[Pasted image 20260930184808.png]]

- **En-tête (_Header_) :**
    
    - **Début de trame :** Délimite le commencement de la trame.
        
    - **Adressage :** Contient les adresses physiques (MAC) source et destination.
        
    - **Type :** Spécifie le protocole de couche 3 transporté (ex: IPv4, IPv6).
        
    - **Contrôle :** Définit les services de gestion (QoS, priorité du trafic comme la VoIP).
        
- **Données (_Payload_) :** Le paquet de couche 3 (IP).
    
- **Queue de bande (_Trailer_) :**
    
    - **Détection des erreurs :** Champ **FCS** (_Frame Check Sequence_) calculé via un code **CRC** (_Cyclic Redundancy Check_) pour vérifier l'intégrité des données à la réception.
        
    - **Fin de trame :** Marque la fin du bloc de transmission.
        

#### 2. Adressage de Couche 2

- **Acheminement local :** Les adresses physiques (ex: MAC) ne servent qu'à la livraison **au sein du même segment réseau** (hôte à routeur, routeur à routeur, routeur à hôte).
    
- **Portée des adresses :**
    
    - **Adresses L2 (MAC) :** Changent à chaque tronçon (hop) lorsqu'un routeur réencapsule le paquet.
        
    - **Adresses L3 (IP) :** Restent identiques de la source finale à la destination finale.
        

#### 3. Trames LAN vs WAN 

Le choix du protocole L2 dépend de la topologie réseau et du support physique :

- **Protocole LAN :** Ethernet (802.3) pour le filaire, Wi-Fi (802.11) pour le sans-fil.
    
- **Protocoles WAN traditionnels :** PPP, HDLC, Frame Relay, ATM, X.25.
    

	- Hôte vers routeur![[Pasted image 20260930184943.png]]
	
	- Routeur à routeur![[Pasted image 20260930185120.png]]

> [!remarque] 
>  **Tendance actuelle :** Ethernet remplace progressivement les anciens protocoles sur les réseaux étendus (WAN).

