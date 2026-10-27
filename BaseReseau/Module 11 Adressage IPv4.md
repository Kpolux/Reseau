#Subnetting #adressage #IPv4 
# Module 11 : Adressage **IPv4**

## 1 Structure de l'adresse IPv4

### 1.1 Parties réseau et hôte
voir [[Module 08 Couche reseau#5. Paquet IPv4]]
- **Structure hiérarchique :** 
	- Une adresse IPv4 se compose de 32 bits séparés en 4 octets, divisés en deux parties : la **partie réseau** (plage d'adresse) et la **partie hôte**.      
    
- **Rôle des parties :**          
    - **Partie réseau :** 
	    - Identifie le réseau spécifique ; tous les équipements d'un même réseau doivent partager des bits réseau identiques.
        
    - **Partie hôte :** 
	    - Identifie de façon unique un équipement spécifique au sein de ce réseau.
        


```
Exemple : 192.168.10.10 /24

  [ Partie Réseau (24 bits) ]      [ Partie Hôte (8 bits) ]
   192   .   168   .    10        .           10
11000000 . 10101000 . 00001010    .        00001010
```

### 1.2 Masque de sous-réseau

- **Définition :** 
	- Valeur de 32 bits composée d'une séquence continue de `1` suivie d'une séquence continue de `0`.
    
- **Fonction :** 
	- Indique au système d'exploitation quelle portion de l'adresse IP correspond au réseau et laquelle correspond aux hôtes.
        
- **Attribution sur un hôte :** 
	- Une configuration IP d'hôte requiert au minimum 
		- une adresse IPv4 unique et un masque de sous-réseau 
		- ainsi qu'une passerelle par défaut et des serveurs DNS pour la communication externe.


```
Masque : 255.255.255.0 aka /24
Binaire : C
          |--- Bits Réseau (24) ---||- Bits Hôte (8) -|
```

### 1.3 Longueur de préfixe (Notation CIDR)

- **Définition :** 
	- Représentation concourante du masque exprimée par une barre oblique (`/`) suivie du nombre total de bits mis à `1` dans le masque.
	- Plus facile a retenir et a exprimer 
	- une adresse réseau est souvent appelée *préfixe*  ou *préfixe réseau* 
    

|**Masque de sous-réseau**|**Représentation binaire (32 bits)**|**Longueur de préfixe**|
|---|---|---|
|**255.0.0.0**|`11111111.00000000.00000000.00000000`|**/8**|
|**255.255.0.0**|`11111111.11111111.00000000.00000000`|**/16**|
|**255.255.255.0**|`11111111.11111111.11111111.00000000`|**/24**|
|**255.255.255.128**|`11111111.11111111.11111111.10000000`|**/25**|
|**255.255.255.192**|`11111111.11111111.11111111.11000000`|**/26**|
|**255.255.255.224**|`11111111.11111111.11111111.11100000`|**/27**|
|**255.255.255.240**|`11111111.11111111.11111111.11110000`|**/28**|
|**255.255.255.248**|`11111111.11111111.11111111.11111000`|**/29**|
|**255.255.255.252**|`11111111.11111111.11111111.11111100`|**/30**|

### 1.4 Déterminer le réseau : Opération logiques AND (ET)

- **Principe :** 
	- Un hôte effectue un ET logique bit à bit entre son adresse IPv4 et son masque de sous-réseau pour extraire l'adresse réseau.
    
- **Règles de la table de vérité AND :**
    
    - $1 \text{ AND } 1 = 1$
        
    - $0 \text{ AND } 1 = 0$
        
    - $1 \text{ AND } 0 = 0$
        
    - $0 \text{ AND } 0 = 0$
 

```
Exemple de calcul pour l'hôte 192.168.10.10 /24 :

  IP Hôte :  192.168.10.10   ->  11000000.10101000.00001010.00001010
  Masque  :  255.255.255.0   ->  11111111.11111111.11111111.00000000
  ------------------------------------------------------------------
  AND (ET):  192.168.10.0   ->   11000000.10101000.00001010.00000000
```

### 1.5 Réseau, Hôte et Adresses de Diffusion (_Broadcast_)

Au sein de tout bloc IP, il existe 3 types d'adresses spécifiques :

1. **Adresse Réseau :**
          
    - Représente le réseau dans son ensemble.
        
    - Tous les bits de la partie hôte sont définis à **0**.
        
    - _Ne peut jamais être assignée à une interface d'hôte_.
    
2. **Adresse de Diffusion (_Broadcast_) :**
          
    - Utilisée pour envoyer un paquet à tous les hôtes du même réseau.
        
    - Tous les bits de la partie hôte sont définis à **1**.
        
    - _Ne peut pas être attribuée à un hôte individuel_.    
    
3. **Adresses d'Hôtes Utilisables :**
    
    - Toutes les adresses situées entre l'adresse réseau et l'adresse de diffusion.
        
    - **Première adresse hôte :** Tous les bits hôtes à `0` avec le dernier bit à `1`.
        
    - **Dernière adresse hôte :** Tous les bits hôtes à `1` avec le dernier bit à `0`.
     
    -  Nombre total d'adresse du réseau  - 2(Réseau et Diffusion) 
        

#### Synthèse d'un sous-réseau exemple (`192.168.10.0/24`)

|**Type d'adresse**|**Format Décimal**|**Partie Réseau (24 bits)**|**Partie Hôte (8 bits)**|
|---|---|---|---|
|**Masque**|`255.255.255.0`|`11111111.11111111.11111111`|`00000000`|
|**Adresse Réseau**|`192.168.10.0`|`11000000.10101000.00001010`|**`00000000`** (Tous 0)|
|**1ère IP Utilisable**|`192.168.10.1`|`11000000.10101000.00001010`|**`00000001`**|
|**Dernière IP Utilisable**|`192.168.10.254`|`11000000.10101000.00001010`|**`11111110`**|
|**Adresse Broadcast**|`192.168.10.255`|`11000000.10101000.00001010`|**`11111111`** (Tous 1)|


## 2 Adresses IPv4 de monodiffusion, de diffusion et de multidiffusion

### 2.1 Monodiffusion (*Unicast*)

* **Définition :** 
	* Communication un-à-un (*one-to-one*) entre un unique émetteur et un unique destinataire.

* **Adresse IP source :** 
	* Doit **toujours** être une adresse de monodiffusion (un paquet provient d'une seule source).

* **Adresse IP destination :** 
	* Adresse de monodiffusion correspondant à un seul périphérique final.

* **Plage d'adresses d'hôte :** 
	* `0.0.0.0` à `223.255.255.255` (à l'exception des plages réservées ). Les adresses situées au-delà (`224.0.0.0` à `255.255.255.255`) sont exclues de l'affectation directe aux hôtes car elles sont réservées au Multicast(Classe D : `224.0.0.0` à `239.255.255.255`) et à l'expérimentation (Classe E: `240.0.0.0` à `255.255.255.255`). 


### 2.2 Diffusion (*Broadcast*)

* **Définition :** 
	* Communication un-à-tous (*one-to-all*) envoyée à tous les équipements du même domaine de diffusion.

* **Régulation du trafic :** 
	* Par défaut, les routeurs **ne transfèrent pas** les paquets de diffusion (ils délimitent les domaines de diffusion).
* Pas de diffusion en IPv6
* réduire les trafics de diffusion pour ne pas affecter les performances du réseau ou des périphériques 

- Types de diffusions IPv4
	1. **Diffusion limitée (*Limited Broadcast*) :**
		* **Adresse de destination :** `255.255.255.255` (32 bits à `1`).
		* **Portée :** Uniquement le réseau local (*LAN*).

	2. **Diffusion dirigée (*Directed Broadcast*) :**
		* **Adresse de destination :** 
			* Adresse de diffusion du réseau spécifique (ex: `172.16.4.255/24` pour le réseau `172.16.4.0/24`).
		* **Comportement IOS :**
			* Un routeur directement connecté au réseau de destination, le paquet est diffusé sur le réseau de destination.
			*  Désactivée par défaut à partir de Cisco IOS 12.0 (`no ip directed-broadcast`) pour des raisons de sécurité.
	


### 2.3 Multidiffusion (*Multicast*)

* **Définition :** 
	* Communication un-à-un-groupe (*one-to-many*) envoyée uniquement aux hôtes inscrits à un groupe multidiffusion.

* **Plage d'adresses réservée :** 
	* `224.0.0.0` à `239.255.255.255`.

* **Optimisation :** 
	* Réduit le volume de trafic par rapport au broadcast en ciblant exclusivement les hôtes abonnés.
	* hôte IPv4 s'abonne à un groupe de multidiffusion

* **Exemple d'application :** 
	* Les protocoles de routage dynamique comme OSPF utilisent l'adresse réservée `224.0.0.5` pour échanger entre routeurs OSPF.


## 3 Types d'adresses IPv4

### 3.1 Adresses IPv4 publiques et privées

- **Adresses publiques :** 
	- Routées de manière globale sur Internet entre les routeurs des fournisseurs d'accès (FAI).
        
- **Adresses privées (RFC 1918) :** 
	- Destinées à un usage interne dans les entreprises et réseaux domestiques. Elles ne sont **pas routables sur Internet** et sont rejetées par les FAI.
    
##### Plages d'adresses privées [RFC 1918](https://www.rfc-editor.org/info/rfc1918/)

| **Adresse réseau et préfixe** | **Plage d'adresses privées**      | **Masque par défaut** |
| ----------------------------- | --------------------------------- | --------------------- |
| **10.0.0.0 /8**               | `10.0.0.0` à `10.255.255.255`     | `255.0.0.0`           |
| **172.16.0.0 /12**            | `172.16.0.0` à `172.31.255.255`   | `255.240.0.0`         |
| **192.168.0.0 /16**           | `192.168.0.0` à `192.168.255.255` | `255.255.0.0`         |

### 3.2 Routage sur Internet et NAT

- **Problématique :** 
	- En raison de l'épuisement des adresses IPv4, les paquets avec une adresse source privée doivent être traduits avant d'être envoyés sur Internet.
    
- **NAT (_Network Address Translation_) :**
          
    - Convertit les adresses IPv4 privées locales en adresses IPv4 publiques uniques globalement avant d'acheminer le trafic vers l'ISP.
                  
    - S'exécute généralement sur le routeur frontal de la bordure du réseau (comme la DMZ ou le routeur d'accès).




### 3.4 Adresses IPv4 réservées

##### 1. Adresses de bouclage (_Loopback_)

- **Plage :** 
	- `127.0.0.0 /8` (`127.0.0.1` à `127.255.255.254`).
        
- **Usage courant :** 
	- `127.0.0.1`.
    
- **Rôle :** 
	- Utilisées par un hôte pour diriger le trafic vers lui-même afin de tester le bon fonctionnement de la pile protocolaire TCP/IP interne (`ping 127.0.0.1`).
    


##### 2. Adresses lien-local (_Link-Local_ / APIPA - *Automatic Private IP Adressing*)

- **Plage :** 
	- `169.254.0.0 /16` (`169.254.0.1` à `169.254.255.254`).
    
- **Rôle :** 
	- Auto-attribuées automatiquement par le système (ex: Windows APIPA) lorsqu'un client DHCP ne parvient pas à joindre un serveur DHCP.
    
- **Portée :** 
	- Uniquement fonctionnelles pour la communication d'égal à égal (_peer-to-peer_) sur le segment local.
 

### 3.5 Adressage d'héritage par classe (_Classful Addressing_)

Historiquement [(RFC 790](https://www.rfc-editor.org/info/rfc790/)), les adresses IPv4 étaient découpées en classes rigides basées sur les premiers octets :

  ![[Pasted image 20261007175453.png]]

| **Classe** | **Plage du 1er octet**              | **Préfixe par défaut** | **Nb de réseaux** | **Hôtes par réseau** | **Usage historique**         |
| ---------- | ----------------------------------- | ---------------------- | ----------------- | -------------------- | ---------------------------- |
| **A**      | `0.0.0.0/8` à `127.0.0.0/8`         | **/8**                 | 128               | 16 777 214           | Très grands réseaux          |
| **B**      | `128.0.0.0/16` à `191.255.0.0/16`   | **/16**                | 16 384            | 65 534               | Réseaux de taille moyenne    |
| **C**      | `192.0.0.0/24` à `223.255.255.0/24` | **/24**                | 2 097 152         | 254                  | Petits réseaux               |
| **D**      | `224.0.0.0` à `239.0.0.0`           | N/A                    | N/A               | N/A                  | Multidiffusion (_Multicast_) |
| **E**      | `240.0.0.0` à `255.0.0.0`           | N/A                    | N/A               | N/A                  | Expérimental / Réservé       |
|            |                                     |                        |                   |                      |                              |
> [!Note: ] 
>  L'adressage par classe entraînait un fort gaspillage d'adresses. Il a été remplacé au milieu des années 1990 par l'adressage sans classe **CIDR** (_Classless Inter-Domain Routing_). 
>



### 3.6 Attribution des adresses IP (IANA et RIR)

- **IANA (_Internet Assigned Numbers Authority_) :** Organisme central qui gère et alloue les blocs d'adresses IP globaux aux Registres Internet Régionaux (_RIR_).
    
- **RIR (_Regional Internet Registries_) :** Distribuent les blocs d'adresses IP aux FAI et grandes entreprises selon leur zone géographique :    
      
    - **AfriNIC :** Région Afrique.
        
    - **APNIC :** Région Asie / Pacifique.
        
    - **ARIN :** Amérique du Nord.
        
    - **LACNIC :** Amérique Latine et Caraïbes.
        
    - **RIPE NCC :** Europe, Moyen-Orient et Asie centrale.
![[Pasted image 20261007180710.png]]

## 4 Segmentation du réseau

### 4.1 Domaines de diffusion et segmentation

- **Définition :** 
	- Un domaine de diffusion (_Broadcast Domain_) regroupe tous les périphériques qui reçoivent une trame de diffusion de Couche 2 ou un paquet de diffusion de Couche 3 émis sur le même segment réseau.
    
- **Comportement des équipements :**    
      
    - **Switch (L2) :** 
	    - Transmettent les diffusions sur toutes leurs interfaces actives, sauf celle sur laquelle la diffusion a été reçue.        
        
    - **Routeurs (L3) :** 
	    - Bloquent les diffusions IP et ne les transfèrent pas d'une interface à une autre. Chaque interface de routeur délimite un domaine de diffusion distinct.
        


### 4.2 Problèmes liés aux grands domaines de diffusion

- **Impression sur les performances :** 
	- Un trop grand nombre d'hôtes dans le même domaine de diffusion (ex: 400 utilisateurs dans un réseau `/16`) génère un volume excessif de trafic de diffusion (requêtes ARP, DHCP, etc.).      
    
- **Conséquences :** 
	- Chaque hôte du réseau doit interrompre son processeur pour traiter chaque paquet de diffusion reçu, ce qui ralentit les opérations globales et dégrade les performances.
    
- **Solution :** 
	- Diviser le grand domaine de diffusion en sous-réseaux plus petits (ex: scinder `172.16.0.0/16` en deux sous-réseaux `/24` : `172.16.0.0/24` et `172.16.1.0/24`).
    

```
[ Avant Segmentation ]
Routeur R1 (G0/0) ─── LAN 1: 172.16.0.0/16 (400 utilisateurs - 1 seul domaine de diffusion)

[ Après Segmentation ]
                  ┌─── G0/0 ─── LAN 1: 172.16.0.0/24 (200 utilisateurs)
Routeur R1 ──────┤
                  └─── G0/1 ─── LAN 2: 172.16.1.0/24 (200 utilisateurs)
```

### 4.3 Raisons pour segmenter les réseaux

- **Amélioration des performances :** 
	- Réduit le trafic global et le traitement des diffusions sur chaque appareil.
    
- **Sécurité renforcée :** 
	- Permet d'isoler les flux sensibles et d'appliquer des politiques de sécurité (comme des listes de contrôle d'accès) entre les sous-réseaux.
    
- **Confinement des erreurs :** 
	- Limite la portée des pannes ou des tempêtes de diffusion (_broadcast storms_) causées par du matériel défectueux ou des attaques.
    
- Critères de structuration des sous-réseaux
	Les administrateurs peuvent regrouper les sous-réseaux selon plusieurs approches organisationnelles : 
	- **Par emplacement physique** : Un sous-réseau dédié par bâtiment, étage ou zone géographique.
	- **Par groupe / fonction** : selon leur service d'appartenance.
	- **Par type d'appareil** : Séparation selon la nature des équipements réseau (Serveur /routeur )

## 5. Sous-réseau  (Subnetting)
- **Subnetting** : Technique qui consiste à diviser un grand réseau IP en plusieurs sous-réseaux plus petits et logiques

- **Principe de base :**  Pour créer des sous-réseaux, on emprunte un ou plusieurs bits  hôte pour les transformer en bits réseau (extension du masque de sous-réseau).

- **Impact** : Plus le nombre de bits empruntés est grand, plus le nombre de sous-réseaux augmente, mais plus le nombre d'hôtes disponibles par sous-réseau diminue. (Gâteau à partager en part égale)


> [!WARNING] Choix du bloc d'adresse privé racine
> 
> Pour concevoir un plan d'adressage IPv4 efficace et évolutif, la première étape consiste à sélectionner un bloc d'adresses privées **suffisamment grand** pour couvrir la totalité du besoin en hôtes, puis à procéder au découpage.

### 5.1 Sur une limite d'octet
C'est ce qu'il y a de plus simple car on voit la séparation directement en décimal.

- On  emprunte un ou plusieurs bloc de 8 bits (octet) hôte pour les transformer en bits réseau.
     
- **Limites d'octets standards (/8, /16, /24) :**
    Placer la coupure exacte entre réseau et hôtes au niveau d'un point (`.`), c'est-à-dire après 8 /16/24 bits
    
| **Longueur de préfixe** | **Masque de sous-réseau** | **Structure binaire (n=network, h=hoˆte)** | **Nb d'hôtes utilisables** |
| ----------------------- | ------------------------- | ------------------------------------------ | -------------------------- |
| **/8**                  | `255.0.0.0`               | `nnnnnnnn.hhhhhhhh.hhhhhhhh.hhhhhhhh`      | 16 777 214                 |
| **/16**                 | `255.255.0.0`             | `nnnnnnnn.nnnnnnnn.hhhhhhhh.hhhhhhhh`      | 65 534                     |
| **/24**                 | `255.255.255.0`           | `nnnnnnnn.nnnnnnnn.nnnnnnnn.hhhhhhhh`      | 254                        |

#### Exemple 1 : Découpage du bloc `10.0.0.0/8` à la limite /16
On décide de faire la coupure au 2e octet (`10.X.0.0 /16`).
- **Bits empruntés :** On passe de `/8` à `/16`, soit **8 bits empruntés** dans le 2e octet.
    
- **Nombre de sous-réseaux :** $2^8 = 256$ sous-réseaux possibles (`10.0.0.0`, `10.1.0.0`, `10.2.0.0`, ... `10.255.0.0`).
    
- **Hôtes par sous-réseau :** Il reste 16 bits hôtes (les 3e et 4e octets), soit $2^{16} - 2 = 65\ 534$ hôtes par sous-réseau.

|**Adresse de sous-réseau**|**Plage d'adresses d'hôtes utilisables**|**Adresse de diffusion (Broadcast)**|
|---|---|---|
|**10.0.0.0 /16**|`10.0.0.1` – `10.0.255.254`|`10.0.255.255`|
|**10.1.0.0 /16**|`10.1.0.1` – `10.1.255.254`|`10.1.255.255`|
|**10.2.0.0 /16**|`10.2.0.1` – `10.2.255.254`|`10.2.255.255`|
|...|...|...|
|**10.255.0.0 /16**|`10.255.0.1` – `10.255.255.254`|`10.255.255.255`|

#### Exemple 2 : Découpage du bloc `10.0.0.0/8` à la limite /24

On décide d'aller plus loin et de faire la coupure au 3e octet (`10.X.Y.0 /24`).

- **Bits empruntés :** On passe de `/8` à `/24`, soit **16 bits empruntés** (les 2e et 3e octets).
    
- **Nombre de sous-réseaux :** $2^{16} = 65\ 536$ sous-réseaux possibles (`10.0.0.0`, `10.0.1.0`, `10.0.2.0`, ..., `10.255.255.0`).
    
- **Hôtes par sous-réseau :** Il ne reste que 8 bits hôtes (le 4e octet), soit $2^8 - 2 = 254$ hôtes par sous-réseau.

|**Adresse de sous-réseau**|**Plage d'adresses d'hôtes utilisables**|**Adresse de diffusion (Broadcast)**|
|---|---|---|
|**10.0.0.0 /24**|`10.0.0.1` – `10.0.0.254`|`10.0.0.255`|
|**10.0.1.0 /24**|`10.0.1.1` – `10.0.1.254`|`10.0.1.255`|
|**10.0.2.0 /24**|`10.0.2.1` – `10.0.2.254`|`10.0.2.255`|
|...|...|...|
|**10.255.255.0 /24**|`10.255.255.1` – `10.255.255.254`|`10.255.255.255`|
 
### 5.2 Sous-réseau à l'intérieur d'une limite d'octet

> [!INFO] 
> 
> 
Lorsqu'un masque `/24` est découpé dans le 4ème octet, chaque bit emprunté **double le nombre de sous-réseaux** tout en réduisant de moitié le nombre d'hôtes disponibles par sous-réseau.
  

#### Découpage d'un réseau `/24`

| **Préfixe** | **Masque de sous-réseau** | **Masque binaire (4ème octet)** | **Bits empruntés** | **Nb de sous-réseaux (2n)** | **Nb d'hôtes utilisables (2h−2)** |
| ----------- | ------------------------- | ------------------------------- | ------------------ | --------------------------- | --------------------------------- |
| **/25**     | `255.255.255.128`         | `10000000`                      | 1                  | **2**                       | **126**                           |
| **/26**     | `255.255.255.192`         | `11000000`                      | 2                  | **4**                       | **62**                            |
| **/27**     | `255.255.255.224`         | `11100000`                      | 3                  | **8**                       | **30**                            |
| **/28**     | `255.255.255.240`         | `11110000`                      | 4                  | **16**                      | **14**                            |
| **/29**     | `255.255.255.248`         | `11111000`                      | 5                  | **32**                      | **6**                             |
| **/30**     | `255.255.255.252`         | `11111100`                      | 6                  | **64**                      | **2**                             |

- **Formules d'adressage fondamentales :**    
      
    - **Nombre de sous-réseaux créés :** $2^n$ (où $n$ = nombre de bits empruntés à la partie hôte).
                
    - **Nombre d'hôtes utilisables par sous-réseau :** $2^h - 2$ (où $h$ = nombre de bits hôtes restants ; le $-2$ retire l'adresse réseau et l'adresse de broadcast).

##### Exemple Décomposition `/24` en 4 Sous-Réseau 

- **Nombre de sous-réseaux
	- 4 = $2^2$  => 2 bits à prendre  
	- 24+2 = 26 => /26
	- MAsk : `11111111.11111111.11111111.11000000` => `255.255.255.192`
	
-  **Nombre d'adresses
	- 32 - 26 = 6 => $2^6$ = 64 adresses 
-  **Nombre d'hôtes 
	- 64 - 2 =  62 hôtes par sous-réseau
- Différents sous-réseaux 
	

```
4e octet :[ Bit 1   Bit 2 ] [ Bit 3   Bit 4   Bit 5   Bit 6   Bit 7   Bit 8 ]
          |--- Réseau ----| |------------------- Hôte ----------------------|

SRx 1 :      0       0       0       0       0       0       0       0    = .0
SRx 2 :      0       1       0       0       0       0       0       0    = .64
SRx 3 :      1       0       0       0       0       0       0       0    = .128
SRx 4 :      1       1       0       0       0       0       0       0    = .192
```


#### Découpage d'un réseau `/16`


| **Préfixe** | **Masque de sous-réseau** | **Masque binaire (N=Network, H=hote)** | **Nb de sous-réseaux (2n)** | **Nb d'hôtes utilisables (2h−2)** |
| ----------- | ------------------------- | -------------------------------------- | --------------------------- | --------------------------------- |
| **/17**     | `255.255.128.0`           | `NNNNNNNN.NNNNNNNN.NHHHHHHH.HHHHHHHH`  | **2**                       | **32 766**                        |
| **/18**     | `255.255.192.0`           | `NNNNNNNN.NNNNNNNN.NNHHHHHH.HHHHHHHH`  | **4**                       | **16 382**                        |
| **/19**     | `255.255.224.0`           | `NNNNNNNN.NNNNNNNN.NNNHHHHH.HHHHHHHH`  | **8**                       | **8 190**                         |
| **/20**     | `255.255.240.0`           | `NNNNNNNN.NNNNNNNN.NNNNHHHH.HHHHHHHH`  | **16**                      | **4 094**                         |
| **/21**     | `255.255.248.0`           | `NNNNNNNN.NNNNNNNN.NNNNNHHH.HHHHHHHH`  | **32**                      | **2 046**                         |
| **/22**     | `255.255.252.0`           | `NNNNNNNN.NNNNNNNN.NNNNNNHH.HHHHHHHH`  | **64**                      | **1 022**                         |
| **/23**     | `255.255.254.0`           | `NNNNNNNN.NNNNNNNN.NNNNNNNH.HHHHHHHH`  | **128**                     | **510**                           |
| **/24**     | `255.255.255.0`           | `NNNNNNNN.NNNNNNNN.NNNNNNNN.HHHHHHHH`  | **256**                     | **254**                           |
| **/25**     | `255.255.255.128`         | `NNNNNNNN.NNNNNNNN.NNNNNNNN.NHHHHHHH`  | **512**                     | **126**                           |
| **/26**     | `255.255.255.192`         | `NNNNNNNN.NNNNNNNN.NNNNNNNN.NNHHHHHH`  | **1 024**                   | **62**                            |
| **/27**     | `255.255.255.224`         | `NNNNNNNN.NNNNNNNN.NNNNNNNN.NNNHHHHH`  | **2 048**                   | **30**                            |
| **/28**     | `255.255.255.240`         | `NNNNNNNN.NNNNNNNN.NNNNNNNN.NNNNHHHH`  | **4 096**                   | **14**                            |
| **/29**     | `255.255.255.248`         | `NNNNNNNN.NNNNNNNN.NNNNNNNN.NNNNNHHH`  | **8 192**                   | **6**                             |
| **/30**     | `255.255.255.252`         | `NNNNNNNN.NNNNNNNN.NNNNNNNN.NNNNNNHH`  | **16 384**                  | **2**                             |

##### Exemple  Créer 100 sous-réseaux avec un préfixe /16 
172.16.0.0/16 
- **Nombre de sous-réseaux
	-  $2^6 = 64$ sous-réseaux (insuffisant).                        
    - $2^7 = 128$ sous-réseaux (satisfait l'exigence de 100 sous-réseaux) => 7 bits à prendre 
    - 16+7 = 23 => /23
    - Mask :  `11111111.11111111.11111110.00000000` => `255.255.254.0`
    
-  **Nombre d'adresses
	- 32 - 23= 9 => $2^9$ = 512 adresses 
-  **Nombre d'hôtes 
	- 512 - 2 =  510 hôtes par sous-réseau
- Différents sous-éseaux  
	- Les **7 premiers bits** (de gauche à droite) du 3ᵉ octet deviennent des bits d'identification du  sous-réseau et évoluent de `0000000` à `1111111`.
	
	- Le **8ᵉ bit** (le plus à droite du 3ᵉ octet) reste un bit hôte. Lors de l'identification du sous-réseau, ce bit hôte est fixé à **`0`**.
    

```
3e octet binaire  | Valeur décimale |
------------------+-----------------+
  0 0 0 0 0 0 0 0 |        0        |
  0 0 0 0 0 0 1 0 |        2        |
  0 0 0 0 0 1 0 0 |        4        |
  0 0 0 0 0 1 1 0 |        6        |
  ...             |       ...       |
  1 1 1 1 1 1 1 0 |      254        |
```


```

3e octet     :  [ B1 B2 B3 B4 B5 B6 B7 ] [ B8 ]  .  [ 8 bits Hôte (4e octet) ]

SRx 1   :         0  0  0  0  0  0  0      0            0.0     -> 172.16.0.0/23
SRx 2   :         0  0  0  0  0  0  1      0            0.0     -> 172.16.2.0/23
SRx 3   :         0  0  0  0  0  1  0      0            0.0     -> 172.16.4.0/23
SRx 4   :         0  0  0  0  0  1  1      0            0.0     -> 172.16.6.0/23
...                                ...
SRx 128 :         1  1  1  1  1  1  1      0            0.0     -> 172.16.254.0/23
```


- Plage d'adresses du premier sous-réseau (`172.16.0.0/23`)

|**Type d'adresse**|**Format Binaire (3e et 4e octets)**|**Format Décimal**|
|---|---|---|
|**Adresse Réseau**|`00000000.00000000`|`172.16.0.0/23`|
|**1ère IP Utilisable**|`00000000.00000001`|`172.16.0.1/23`|
|**Dernière IP Utilisable**|`00000001.11111110`|`172.16.1.254/23`|
|**Adresse Broadcast**|`00000001.11111111`|`172.16.1.255/23`|

#### Découpage d'un réseau /8

| **Préfixe** | **Masque de sous-réseau** | **Masque binaire (N=Network, H=Hoˆte)** | **Nb de sous-réseaux (2n)** | **Nb d'hôtes utilisables (2h−2)** |
| ----------- | ------------------------- | --------------------------------------- | --------------------------- | --------------------------------- |
| **/9**      | `255.128.0.0`             | `NNNNNNNN.NHHHHHHH.HHHHHHHH.HHHHHHHH`   | 2                           | 8 388 606                         |
| **/10**     | `255.192.0.0`             | `NNNNNNNN.NNHHHHHH.HHHHHHHH.HHHHHHHH`   | 4                           | 4 194 302                         |
| **/11**     | `255.224.0.0`             | `NNNNNNNN.NNNHHHHH.HHHHHHHH.HHHHHHHH`   | 8                           | 2 097 150                         |
| **/12**     | `255.240.0.0`             | `NNNNNNNN.NNNNHHHH.HHHHHHHH.HHHHHHHH`   | 16                          | 1 048 574                         |
| **/13**     | `255.248.0.0`             | `NNNNNNNN.NNNNNHHH.HHHHHHHH.HHHHHHHH`   | 32                          | 524 286                           |
| **/14**     | `255.252.0.0`             | `NNNNNNNN.NNNNNNHH.HHHHHHHH.HHHHHHHH`   | 64                          | 262 142                           |
| **/15**     | `255.254.0.0`             | `NNNNNNNN.NNNNNNNH.HHHHHHHH.HHHHHHHH`   | 128                         | 131 070                           |
| **/16**     | `255.255.0.0`             | `NNNNNNNN.NNNNNNNN.HHHHHHHH.HHHHHHHH`   | 256                         | 65 534                            |
| **/17**     | `255.255.128.0`           | `NNNNNNNN.NNNNNNNN.NHHHHHHH.HHHHHHHH`   | 512                         | 32 766                            |
| **/18**     | `255.255.192.0`           | `NNNNNNNN.NNNNNNNN.NNHHHHHH.HHHHHHHH`   | 1 024                       | 16 382                            |
| **/19**     | `255.255.224.0`           | `NNNNNNNN.NNNNNNNN.NNNHHHHH.HHHHHHHH`   | 2 048                       | 8 190                             |
| **/20**     | `255.255.240.0`           | `NNNNNNNN.NNNNNNNN.NNNNHHHH.HHHHHHHH`   | 4 096                       | 4 094                             |
| **/21**     | `255.255.248.0`           | `NNNNNNNN.NNNNNNNN.NNNNNHHH.HHHHHHHH`   | 8 192                       | 2 046                             |
| **/22**     | `255.255.252.0`           | `NNNNNNNN.NNNNNNNN.NNNNNNHH.HHHHHHHH`   | 16 384                      | 1 022                             |
| **/23**     | `255.255.254.0`           | `NNNNNNNN.NNNNNNNN.NNNNNNNH.HHHHHHHH`   | 32 768                      | 510                               |
| **/24**     | `255.255.255.0`           | `NNNNNNNN.NNNNNNNN.NNNNNNNN.HHHHHHHH`   | 65 536                      | 254                               |
| **/25**     | `255.255.255.128`         | `NNNNNNNN.NNNNNNNN.NNNNNNNN.NHHHHHHH`   | 131 072                     | 126                               |
| **/26**     | `255.255.255.192`         | `NNNNNNNN.NNNNNNNN.NNNNNNNN.NNHHHHHH`   | 262 144                     | 62                                |
| **/27**     | `255.255.255.224`         | `NNNNNNNN.NNNNNNNN.NNNNNNNN.NNNHHHHH`   | 524 288                     | 30                                |
| **/28**     | `255.255.255.240`         | `NNNNNNNN.NNNNNNNN.NNNNNNNN.NNNNHHHH`   | 1 048 576                   | 14                                |
| **/29**     | `255.255.255.248`         | `NNNNNNNN.NNNNNNNN.NNNNNNNN.NNNNNHHH`   | 2 097 152                   | 6                                 |
| **/30**     | `255.255.255.252`         | `NNNNNNNN.NNNNNNNN.NNNNNNNN.NNNNNNHH`   | 4 194 304                   | 2                                 |

##### Exemple : Créer 1000 sous-réseaux avec un préfixe /8 (`10.0.0.0/8`)

- **Nombre de sous-réseaux :**
          
    - $2^9 = 512$ sous-réseaux (insuffisant)
        
    - $2^{10} = 1024$ sous-réseaux (satisfait l'exigence de 1000 sous-réseaux) $\implies \mathbf{10 \text{ bits}}$ à prendre.
        
    - $8 + 10 = 18 \implies \mathbf{/18}$
        
    - **Masque :** `11111111.11111111.11000000.00000000` $\implies \mathbf{255.255.192.0}$
        
- **Nombre d'adresses :**      
    - $32 - 18 = 14 \implies 2^{14} = \mathbf{16\ 384 \text{ adresses}}$
            
- **Nombre d'hôtes :**
      
    - $16\ 384 - 2 = \mathbf{16\ 382 \text{ hôtes par sous-réseau}}$
        
           
- **Différents sous-réseaux :**    
      
    - Les 10 premiers bits (de gauche à droite) des 2ᵉ et 3ᵉ octets deviennent des bits d'identification du sous-réseau et évoluent de `0000000000` à `1111111111`.
          
        
    - Les 6 derniers bits du 3ᵉ octet et les 8 bits du 4ᵉ octet restent des bits hôtes. Lors de l'identification du sous-réseau, ces bits hôtes sont fixés à `0`.
        

#### Premiers sous-réseaux /18 générés

```
SRx 1    : 00000000 . 00 000000 . 00000000 -> 10.0.0.0/18
SRx 2    : 00000000 . 01 000000 . 00000000 -> 10.0.64.0/18
SRx 3    : 00000000 . 10 000000 . 00000000 -> 10.0.128.0/18
SRx 4    : 00000000 . 11 000000 . 00000000 -> 10.0.192.0/18
SRx 5    : 00000001 . 00 000000 . 00000000 -> 10.1.0.0/18
...
SRx 1024 : 11111111 . 11 000000 . 00000000 -> 10.255.192.0/18
```

#### Plage d'adresses du premier sous-réseau (`10.0.0.0/18`)

| **Type d'adresse**         | **Format Binaire (du 2ᵉ au 4ᵉ octet)** | **Format Décimal** |
| -------------------------- | -------------------------------------- | ------------------ |
| **Adresse Réseau**         | `00000000.00000000.00000000`           | `10.0.0.0/18`      |
| **1ère IP Utilisable**     | `00000000.00000000.00000001`           | `10.0.0.1/18`      |
| **Dernière IP Utilisable** | `00000000.00111111.11111110`           | `10.0.63.254/18`   |
| **Adresse Broadcast**      | `00000000.00111111.11111111`           | `10.0.63.255/18`   |

> [!TIP] Astuce : Calcul direct du $N$-ième sous-réseau
> 
> Pour trouver rapidement l'adresse d'un **$N$-ième sous-réseau** sans dérouler toute la liste :
> 
> ### 1. Méthode Binaire Universelle (Tous masques)
> 1. Calculez l'index : $\text{Index} = N - 1$
> 2. Convertissez cet $\text{Index}$ en binaire sur **$n$ bits** ($n$ = nombre de bits empruntés).
> 3. Injectez ces $n$ bits à l'emplacement exact des bits de sous-réseau.
> 
> ---
> 
> ### 2. Méthode Décimale Rapide (Cas du /18 sur un /8)
> Pour un découpage **/18** ($10 \text{ bits empruntés}$), le 3ᵉ octet avance par pas de **64** (`0`, `64`, `128`, `192`), soit $4$ sous-réseaux par bloc du 2ᵉ octet :
> 
> $$\text{Index} = N - 1$$
> - **2ᵉ octet** = $\lfloor \text{Index} / 4 \rfloor$ *(Quotient entiers)*
> - **3ᵉ octet** = $(\text{Index} \bmod 4) \times 64$ *(Reste de la division $\times 64$)*
> 
> > [!EXAMPLE] Exemple : Trouver le **102ᵉ sous-réseau**
> > 1. $\text{Index} = 102 - 1 = \mathbf{101}$
> > 2. $101 / 4 = \mathbf{25}$ (reste $1$)
> > 3. **2ᵉ octet** = $25$
> > 4. **3ᵉ octet** = $1 \times 64 = 64$
> > 
> > $\implies$ **`10.25.64.0/18`**

---

> [!TIP] Astuce : La méthode du Magic Number (Nombre Magique)
> 
> Le **Magic Number** correspond au pas de progression (la taille du bloc) dans l'octet où se situe la coupure du masque.
> 
> ### 1. La Formule Unique
> $$\text{Magic Number} = 256 - \text{Valeur décimale de l'octet modifié du masque}$$
> 
> Alternativement, avec les bits hôtes $h_{\text{octet}}$ restants dans cet octet :
> $$\text{Magic Number} = 2^{h_{\text{octet}}}$$
> 
> ---
> 
> ### 2. Les 3 Règles d'Or du Magic Number
> 
> 1. **Repérer l'octet modifié :** C'est l'octet qui contient une valeur différente de `255` ou `0` dans le masque (ex. dans `255.255.240.0`, c'est le **3ᵉ octet**).
> 2. **Calculer le Magic Number ($M$) :** $256 - 240 = \mathbf{16}$.
> 3. **Appliquer le pas de progression ($M$) :** Les adresses réseau progressent par multiples de $M$ dans cet octet (`.0`, `.16`, `.32`, `.48`, etc.).
> 
> ---
> 
> ### 3. Trouver la plage complète d'un sous-réseau
> 
> Une fois le Magic Number ($M$) connu pour un sous-réseau donné :
> 
> * **Adresse Réseau :** Le plus grand multiple de $M$ inférieur ou égal à l'IP donnée.
> * **Prochain Réseau :** Multiplier le bloc actuel par $+ M$.
> * **Adresse de Broadcast :** (Adresse du réseau suivant) $- 1$.
> * **Plage d'hôtes utilisables :** Entre l'adresse réseau $+ 1$ et l'adresse de broadcast $- 1$.
> 
> ---
> 
> > [!EXAMPLE] Exemple concret : Trouver le sous-réseau de l'IP `192.168.1.150 /27`
> > 
> > 1. **Masque décimal :** `/27` $\implies$ `255.255.255.224` (l'octet modifié est le **4ᵉ octet**).
> > 2. **Calcul du Magic Number :** $M = 256 - 224 = \mathbf{32}$.
> > 3. **Trouver le réseau :** Multiples de 32 $\implies 0, 32, 64, 96, 128, 160...$
> >    * Pour l'IP `.150`, le multiple le plus proche sans dépasser est **128**.
> > 
> > **Résultat instantané :**
> > * **Adresse Réseau :** `192.168.1.128 /27`
> > * **Réseau Suivant :** `192.168.1.160` ($128 + 32$)
> > * **Adresse Broadcast :** `192.168.1.159` ($160 - 1$)
> > * **Première IP Utilisable :** `192.168.1.129`
> > * **Dernière IP Utilisable :** `192.168.1.158`




## 6. VLSM (Variable Length Subnet Masking)

### 6.1. Principe et Nécessité du VLSM

Le sous-réseautage classique (*FLSM - Fixed Length Subnet Masking*) impose d'utiliser un masque unique pour l'ensemble des sous-réseaux. Cette approche entraîne un **gaspillage massif d'adresses IP**, en particulier sur les liaisons point-à-point (routeur à routeur) qui n'ont besoin que de **2 adresses IP utilisables**.

> [!ABSTRACT] Définition du VLSM
> Le **VLSM** consiste à appliquer un sous-réseautage à un sous-réseau déjà existant. Il permet d'utiliser des **masques de longueurs différentes** selon les besoins réels de chaque segment (nombre d'hôtes requis).

---
![[Pasted image 20261012234039.png]]

---
![[Pasted image 20261012234207.png]]

---
![[Pasted image 20261012234341.png]]


### 6.2. La Règle d'Or du VLSM

> [!WARNING] Règle fondamentale de conception
> **Toujours trier et allouer les sous-réseaux du PLUS GRAND besoin au PLUS PETIT besoin.**
> 
> Ignorer cette règle entraîne des chevauchements d'adresses (*IP overlaps*) et rend la segmentation incohérente.



### 6.3. Méthodologie pas à pas

1. **Lister les besoins :** Classer chaque segment/service par nombre d'hôtes requis en ordre décroissant.
2. **Ajouter la marge de sécurité :** Ajouter les adresses Réseau et Broadcast ($+2$), puis arrondir à la puissance de 2 supérieure ($2^h$).
3. **Déterminer le masque ($/p$) :** $p = 32 - h$.
4. **Allouer les blocs :** Assigner le premier bloc au réseau de départ, puis calculer le réseau suivant en ajoutant la taille du bloc (le *Magic Number*).



### 6.4. Exemple Pratique Complet

**Réseau racine attribué :** `192.168.10.0 /24` (256 adresses totales)

### Cahier des charges :
* **LAN LAN_Ventes :** 100 hôtes
* **LAN LAN_Compta :** 50 hôtes
* **LAN LAN_Admin :** 20 hôtes
* **Liaison WAN Router1-Router2 :** 2 hôtes



### Étape 1 : Calcul des masques nécessaires

| Segment | Hôtes requis | Formule ($2^h - 2 \ge \text{Besoin}$) | Bits hôtes ($h$) | Taille du bloc ($2^h$) | Préfixe ($32 - h$) | Masque Décimal |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **LAN_Ventes** | 100 | $2^7 - 2 = 126$ | 7 | 128 | **/25** | `255.255.255.128` |
| **LAN_Compta** | 50 | $2^6 - 2 = 62$ | 6 | 64 | **/26** | `255.255.255.192` |
| **LAN_Admin** | 20 | $2^5 - 2 = 30$ | 5 | 32 | **/27** | `255.255.255.224` |
| **Liaison WAN** | 2 | $2^2 - 2 = 2$ | 2 | 4 | **/30** | `255.255.255.252` |

---

### Étape 2 : Plan d'adressage VLSM final

| Nom du Segment | Besoins | Adresse Réseau | Masque | Plage IP Utilisables | Adresse de Broadcast |
| :--- | :---: | :--- | :---: | :--- | :--- |
| **LAN_Ventes** | 100 | `192.168.10.0` | `/25` | `192.168.10.1` – `192.168.10.126` | `192.168.10.127` |
| **LAN_Compta** | 50 | `192.168.10.128` | `/26` | `192.168.10.129` – `192.168.10.186` | `192.168.10.187` |
| **LAN_Admin** | 20 | `192.168.10.188` | `/27` | `192.168.10.189` – `192.168.10.218` | `192.168.10.219` |
| **Liaison WAN** | 2 | `192.168.10.220` | `/30` | `192.168.10.221` – `192.168.10.222` | `192.168.10.223` |
| *(Disponible)* | 32 | `192.168.10.224` | `/27` | Réserve pour extension future | `192.168.10.255` |