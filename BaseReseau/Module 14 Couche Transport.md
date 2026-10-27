
#  **Module 14 : Couche Transport**

Liaison indispensable entre la couche application et les couches inférieures (réseau, accès réseau). Elle assure la communication logique entre les applications exécutées sur différents hôtes, sans se soucier du type de support ou du chemin emprunté.


##  **1. Responsabilités clés de la couche transport**

- **Tracking / Suivi des conversations individuelles :** 
	- Garantit le suivi séparé de chaque flux de données circulant entre une application source et une destination (ex. navigateur web, e-mail, streaming simultanés).
    
- **Segmentation et réassemblage :** 
	- Divise les données applicatives en blocs plus petits (segments ou datagrammes) faciles à transporter, puis les reconstruit à la réception.
    
- **Ajout d'en-têtes :** 
	- Intègre des champs de contrôle permettant aux protocoles récepteurs de gérer la communication et d'acheminer les données au bon programme.
    
- **Identification des applications :** 
	- Assigne un numéro de port unique à chaque processus applicatif pour transmettre les flux aux bonnes applications cibles (ex. Ports 80/443 pour Web, Ports 110/25 pour E-mail).
    
- **Multiplexage :** 
	- Entrelace plusieurs conversations sur le même réseau physique afin que plusieurs applications puissent utiliser le réseau simultanément sans bloquer la bande passante.



##  **2. Comparatif TCP vs UDP**

Pour répondre aux exigences variées des applications, la couche transport propose deux protocoles fondamentaux : **TCP** et **UDP**.

```
                 Modèle TCP/IP (Couche Transport)
                                |
        +-----------------------+-----------------------+
        |                                               |
     🤝 TCP                                          ⚡ UDP
(Connexion & Fiabilité)                         (Vitesse & Faible Surcharge)

```


| **Critère**            | **TCP**                               | **UDP**                            |
| ---------------------- | ------------------------------------- | ---------------------------------- |
| **Mode**               | Orienté connexion (session préalable) | Sans connexion                     |
| **Fiabilité**          | Garantie (ACK + retransmissions)      | Aucune garantie (_Best effort_)    |
| **Gestion des pertes** | Retransmission des segments perdus    | Aucune retransmission              |
| **Ordre des données**  | Reconstitution séquentielle           | Aucun réordonnancement             |
| **Surcharge**          | Élevée (en-tête volumineux)           | Très faible (en-tête léger)        |
| **Usage typique**      | Web (HTTP/S), E-mail, Fichiers (FTP)  | Voix (VoIP), Vidéo temps réel, DNS |
  

## **3. Fonctions clés  TCP**

- **Établissement de session :** 
	- Protocole orienté connexion (session négociée avant la transmission).
    
- **Livraison fiable :** 
	- Garantie de réception de chaque segment (retransmission en cas de perte).
    
- **Reconstitution séquentielle :** 
	- Numérotation des segments pour un réassemblage dans l'ordre exact.
      
- **Contrôle de flux :** 
	- Régulation du débit d'envoi selon la capacité du destinataire pour éviter la saturation.
    

### **3.1. En-tête TCP (20 octets de surcharge)**

TCP est un protocole **avec état** qui enregistre les segments envoyés et reçus.


```
	 0                   1                   2                   3   
	 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|          Port source (16)     |      Port de destination (16) |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|                    Numéro de séquence (32)                    |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|                Numéro d'accusé de réception (32)              |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	| Longueur | Réservé |  Bits de  |          Fenêtre (16)        |
	| en-tête  |  (6)    | contrôle  |                              |
	|   (4)    |         |   (6)     |                              |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|     Somme de contrôle (16)    |          Urgent (16)          |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|                    Options (0 ou 32 bits)                     |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```


- **Ports (16 bits chacun) :** 
	- Identifient les applications source et destination.  
    
- **Numéro d'ordre (32 bits)** :
	- Permet le réassemblage séquentiel des données.  
    
- **Numéro d'accusé de réception (32 bits) :** 
	- Indique les données reçues et le prochain octet attendu.
      
- **Bits de contrôle (6 bits) :** 
	- Drapeaux (_flags_) gérant l'état de la session (ex. SYN, ACK, FIN).
      
- **Taille de fenêtre (16 bits) :** 
	- Règle le volume de données acceptables à un instant donné (contrôle de flux).
      
- **Somme de contrôle (16 bits) :** 
	- Vérifie l'intégrité de l'en-tête et des données.
    
### Encapsulation dans un paquet IPv4 (IPv4 + TCP)

Taille minimale des en-têtes cumulés :  
	**40 octets** (20 octets IPv4 + 20 octets TCP).
	


```
	+-------------------------------------------------------------------+
	|                        EN-TÊTE IPv4 (20 octets)                   |
	|  - Adresse IP Source & Destination                                |
	|  - Protocole = 6 (indique que le contenu est du TCP)              |
	+-------------------------------------------------------------------+
	|                        EN-TÊTE TCP (20 octets)                    |
	|  - Port Source & Port Destination                                 |
	|  - Numéro de séquence (32 bits)                                   |
	|  - Numéro d'accusé de réception (32 bits)                         |
	|  - Longueur (4b) | Réservé (6b) | Flags (6b) | Fenêtre (16 bits)  |
	|  - Somme de contrôle (16 bits) | Pointeur urgent (16 bits)        |
	+-------------------------------------------------------------------+
	|                        DONNÉES APPLICATIVES                       |
	|  (HTTP, HTTPS, FTP, SMTP, SSH, etc.)                              |
	+-------------------------------------------------------------------+
```

### Encapsulation dans un paquet IPv6 (IPv6 + TCP)

Taille fixe des en-têtes cumulés : 
	**60 octets** (40 octets IPv6 + 20 octets TCP).


```
	+-------------------------------------------------------------------+
	|                        EN-TÊTE IPv6 (40 octets)                   |
	|  - Adresse IPv6 Source & Destination (128 bits chacune)           |
	|  - En-tête suivant (Next Header) = 6 (indique le protocole TCP)   |
	+-------------------------------------------------------------------+
	|                        EN-TÊTE TCP (20 octets)                    |
	|  - Port Source & Port Destination                                 |
	|  - Numéro de séquence (32 bits)                                   |
	|  - Numéro d'accusé de réception (32 bits)                         |
	|  - Longueur (4b) | Réservé (6b) | Flags (6b) | Fenêtre (16 bits)   |
	|  - Somme de contrôle (16 bits) | Pointeur urgent (16 bits)        |
	+-------------------------------------------------------------------+
	|                        DONNÉES APPLICATIVES                       |
	|  (HTTP, HTTPS, FTP, SMTP, SSH, etc.)                              |
	+-------------------------------------------------------------------+
```


### **3.2. Applications TCP courantes**

Utilisé par les services exigeant une fiabilité absolue sans perte de données :

- **HTTP / HTTPS (80 / 443) :** 
	- HyperText Transfer Protocol / HyperText Transfer Protocol Secure (sécurisé avec chiffrement)
	- Navigation web 
    
- **FTP (20 / 21):** 
	- File Transfer Protocol
	- Transfert de fichiers
      
- **SMTP(25) :** 
	- Simple Mail Transfer Protocol
	- Envoi d'e-mails  
    
- **SSH (22):** 
	- Secure Shell
	- Administration à distance sécurisée

  

## **4. Caractéristiques principales d'UDP**

UDP est un protocole **sans état** (_stateless_) et léger, axé sur la rapidité de transmission (_Best Effort_) :


- **Aucun établissement de session :** 
	- Envoie les datagrammes sans poignée de main préalable.
      
- **Pas de retransmission :** 
	- Les segments perdus ne sont pas réexpédiés.
    
- **Reconstitution à l'arrivée :** 
	- Les données sont traitées dans l'ordre de leur réception, sans réordonnancement.
       
- **Gestion des ressources :** 
	- L'expéditeur n'est pas informé de la disponibilité du récepteur.
    

### **4.1. Structure de l'en-tête UDP (8 octets)**

L'en-tête est très simple et ne comporte que 4 champs de 16 bits chacun (64 bits au total) :


```
	 0                   1                   2                   3   
	 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|          Port source (16)     |      Port de destination (16) |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
	|           Longueur (16)       |      Somme de contrôle (16)   |
	+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

- **Port source (16 bits) :** 
	- Identifie l'application émettrice.
        
- **Port de destination (16 bits) :** 
	- Identifie l'application réceptrice.
    
- **Longueur (16 bits) :** 
	- Indique la taille totale de l'en-tête et des données UDP.
    
- **Somme de contrôle / Checksum (16 bits) :** 
	- Vérifie l'absence d'erreurs dans le datagramme.

#### Encapsulation IPv4 + UDP

Taille minimale des en-têtes cumulés : 
	**28 octets** (20 octets IPv4 + 8 octets UDP).


```
	+-------------------------------------------------------------------+
	|                        EN-TÊTE IPv4 (20 octets)                   |
	|  - Adresse IP Source & Destination                                |
	|  - Protocole = 17 (indique que le contenu est du UDP)             |
	+-------------------------------------------------------------------+
	|                        EN-TÊTE UDP (8 octets)                     |
	|  - Port Source (16 bits)     | Port Destination (16 bits)         |
	|  - Longueur (16 bits)        | Somme de contrôle (16 bits)        |
	+-------------------------------------------------------------------+
	|                        DONNÉES APPLICATIVES                       |
	|  (VoIP, DNS, DHCP, TFTP, SNMP, etc.)                              |
	+-------------------------------------------------------------------+
```

#### Encapsulation IPv6 + UDP

Taille fixe des en-têtes cumulés : 
	**48 octets** (40 octets IPv6 + 8 octets UDP).


```
	+-------------------------------------------------------------------+
	|                        EN-TÊTE IPv6 (40 octets)                   |
	|  - Adresse IPv6 Source & Destination (128 bits chacune)           |
	|  - En-tête suivant (Next Header) = 17 (indique le protocole UDP)  |
	+-------------------------------------------------------------------+
	|                        EN-TÊTE UDP (8 octets)                     |
	|  - Port Source (16 bits)     | Port Destination (16 bits)         |
	|  - Longueur (16 bits)        | Somme de contrôle (16 bits)        |
	+-------------------------------------------------------------------+
	|                        DONNÉES APPLICATIVES                       |
	|  (VoIP, DNS, DHCP, TFTP, SNMP, etc.)                              |
	+-------------------------------------------------------------------+
```




### **4.2. Applications UDP **

UDP est privilégié dans trois scénarios principaux :

1. **Vidéo et multimédia en direct (Tolérance aux pertes, besoin de vitesse)** :    
      
    - **VoIP :** 
	    - _Voice over IP_ (Téléphonie sur IP).          
        
    - **Visioconférence :** 
	    - Streaming vidéo en temps réel.
        
2. **Transactions simples de type demande / réponse** :      
    - **DNS (Port 53):** 
	    - _Domain Name System_ (Résolution de noms de domaine).      
            
    - **DHCP(Ports 67/68) :** 
	    - _Dynamic Host Configuration Protocol_ (Configuration IP dynamique).
          
3. **Applications gérant elles-mêmes la fiabilité** :    
      
    - **SNMP (Port 161) :** 
	    - _Simple Network Management Protocol_ (Gestion et supervision réseau). 
        
    - **TFTP(Port 69) :** 
	    - _Trivial File Transfer Protocol_ (Transfert de fichiers simplifié).


  
## **5.Numéros de port**
### **5.1. Rôle et notion de Socket / Interface de connexion**

- **Port source :** 
	- Généré dynamiquement par le client (ex. 1099, 1305) pour identifier de façon unique chaque conversation.      
    
- **Port de destination :** 
	- Identifie le service demandé sur le serveur (ex. 80 pour le Web, 21 pour le FTP).
      
    
- **Socket / Interface de connexion :** 
	- Combinaison de l'adresse IP et du numéro de port (ex. `192.168.1.5:1099`).
    
     
- **Paire de sockets :** 
	- Associe la socket source et la socket destination (ex. `192.168.1.5:1099 <-> 192.168.1.7:80`) pour différencier les flux simultanés.
    
  

### **5.2. Les 3 groupes de ports (Gérés par l'IANA)**

- **Ports connus (_Well-Known_) [0 à 1023] :** Réservés aux services universels (HTTP, SSH, SMTP).
    
- **Ports inscrits (_Registered_) [1024 à 49151] :** Attribués à des applications spécifiques d'éditeurs (ex. RADIUS : 1812).
    
- **Ports dynamiques / Éphémères [49152 à 65535] :** Assignés temporairement par le client lors de l'ouverture d'une session.

### **5.3. Principaux ports connus 

> [!danger] **à retenir (CCNA 1)**
>

| **Port**    | **Protocole** | **Application / Service**                     |
| ----------- | ------------- | --------------------------------------------- |
| **20 / 21** | TCP           | **FTP** (20 : Données / 21 : Contrôle)        |
| **22**      | TCP           | **SSH** (Connexion distante sécurisée)        |
| **23**      | TCP           | **Telnet** (Connexion distante non sécurisée) |
| **25**      | TCP           | **SMTP** (Envoi de courriels)                 |
| **53**      | UDP / TCP     | **DNS** (Résolution de noms)                  |
| **67 / 68** | UDP           | **DHCP** (67 : Serveur / 68 : Client)         |
| **69**      | UDP           | **TFTP** (Transfert de fichiers simple)       |
| **80**      | TCP           | **HTTP** (Web non chiffré)                    |
| **110**     | TCP           | **POP3** (Réception de courriels)             |
| **143**     | TCP           | **IMAP** (Consultation de courriels)          |
| **161**     | UDP           | **SNMP** (Supervision réseau)                 |
| **443**     | TCP           | **HTTPS** (Web sécurisé / chiffré)            |

### **5.4. Commande de diagnostic et & Audits : 

- Affiche les connexions réseau TCP actives, les adresses IP et les numéros de ports associés.
        
- **Option `-n` :** 
	- Affiche directement les adresses IP et numéros de ports sous forme numérique (évite la résolution de noms).


> [!note]-  **Nmap (Network Mapper)**
> * Outil CLI open-source de référence pour l'exploration réseau, la cartographie et l'audit de sécurité. Il agit principalement au niveau des couches **Réseau (L3)** et **Transport (L4)**.
> * **Attaque par accès** : Contournement de l'authentification ("code du jour") puis injection de commandes système.
> * Nmap envoie des paquets personnalisés ([[TCP]], [[UDP]], [[ICMP]]) vers une cible pour analyser ses réponses et déterminer l'état des ports :
> 	* **Ouvert (*Open*)** : Une application écoute et accepte les connexions sur ce port.
> 	* **Fermé (*Closed*)** : Aucune application n'écoute, mais le port est accessible (réponse `RST` sous TCP).
> 	* **Filtré (*Filtered*)** : Un pare-feu ou un filtre IP empêche Nmap d'atteindre le port.


> [!TIP]- **Nmap (Network : ** Principaux types de Scans
> 1. Scan TCP SYN (`-sS`) — *Stealth Scan / Semi-ouvert*
> 	* Nmap envoie un paquet `SYN`.
> 	* Si la cible répond `SYN-ACK`, le port est **ouvert**. Nmap coupe immédiatement la connexion avec un `RST` au lieu de terminer le *Handshake* (3-way handshake).
> 	* **Avantage** : Rapide et plus discret.
> 2. Scan TCP Connect (`-sT`)
> 	- Établit une connexion TCP complète (`SYN` $\rightarrow$ `SYN-ACK` $\rightarrow$ `ACK`).
> 	- Utilisé lorsque l'utilisateur n'a pas les privilèges administrateur/root.
> 3. Scan TCP Connect (`-sT`)
> 	-  Établit une connexion TCP complète (`SYN` → `SYN-ACK` → `ACK`).
> 	- Utilisé lorsque l'utilisateur n'a pas les privilèges administrateur/root.


#### Example Commandes Utiles

```bash
# Scan rapide des 1000 ports les plus courants
nmap 192.168.1.1

# Détection de l'OS et des versions de services
nmap -sV -O 192.168.1.1

# Scan TCP SYN discret sur une plage de ports spécifique
sudo nmap -sS -p 22,80,443 192.168.1.100

# Scan UDP sur les ports connus (DNS, DHCP, SNMP)
sudo nmap -sU -p 53,67,161 192.168.1.1

# Scan complet de tout le sous-réseau (Ping sweep)
nmap -sn 192.168.1.0/24
````


## 6. Processus de communication TCP

### 6.1. Échange Client-Serveur

- **Attribution des ports :**  
      
    - Chaque processus applicatif s'exécutant sur un serveur utilise un numéro de port spécifique (attribué automatiquement ou configuré par l'administrateur).          
        
    - Deux services ne peuvent pas être affectés au même numéro de port au sein des mêmes services de la couche transport.     
        
    - Un port est considéré comme **ouvert** lorsqu'une application serveur active écoute et accepte les connexions adressées à ce port.
          
        
- **Échange de ports Client / Serveur :**    
      
    - **Requête client :** 
	    - Utilise un port source généré dynamiquement (ex. Port `49152` ou `51152`) et un port de destination connu (_Well-Known_) identifiant le service réseau (ex. Port `80` pour HTTP, Port `25` pour SMTP).          
        
    - **Réponse serveur :** Le serveur inverse les ports de la demande initiale :
          
        - Port source = Port de destination d'origine (ex. Port `80` ou `25`).
            
        - Port de destination = Port source d'origine généré par le client (ex. Port `49152` ou `51152`).
            
              
            

### 6.2. Établissement d'une connexion TCP (Handshake en 3 étapes)

Valide que l'hôte de destination est présent sur le réseau, qu'il dispose d'un service actif sur le port ciblé et l'informe du souhait d'établir une session.

- **Étape 1 — SYN :**         
    - Le client envoie un segment avec le bit de contrôle `SYN` activé (`CTL=SYN`) et un numéro de séquence initial (`SEQ=100`) pour demander l'ouverture de session.
          
        
- **Étape 2 — SYN, ACK :**      
    - Le serveur répond avec les bits `SYN` et `ACK` activés (`CTL=SYN,ACK`), confirme la réception avec `ACK=101` et transmet son propre numéro de séquence initial (`SEQ=300`).
        
        
- **Étape 3 — ACK :**
    - Le client répond avec le bit `ACK` activé (`CTL=ACK`), confirme la réception avec `ACK=301` (`SEQ=101`). La connexion bidirectionnelle est établie.
        
          
        


```
 Client A                                                      Serveur B
    |                                                             |
    |-------------- (1) Envoi de SYN (SEQ=100 CTL=SYN) ---------->| (SYN reçu)
    |                                                             |
    |<-- (2) Envoi de SYN, ACK (SEQ=300 ACK=101 CTL=SYN,ACK) -----|
(SYN, ACK reçu)                                                   |
    |                                                             |
    |-------------- (3) Envoi de ACK (SEQ=101 ACK=301 CTL=ACK) -->|
    |                                                             |
```

### 6.3. Interruption de session TCP (Fermeture en 4 étapes)

Pour clore une connexion, l'indicateur `FIN` (_Finish_) est utilisé. Chaque session unidirectionnelle est fermée séparément (2 étapes par direction, soit 4 étapes au total) :

- **Étape 1 — FIN (Client $\rightarrow$ Serveur) :** 
	- Lorsque le client n'a plus de données à envoyer, il transmet un segment avec l'indicateur `FIN` activé.
    
- **Étape 2 — ACK (Serveur $\rightarrow$ Client) :** 
	- Le serveur accuse réception du segment `FIN` en renvoyant un segment `ACK`.
      
    
- **Étape 3 — FIN (Serveur $\rightarrow$ Client) :** 
	- Le serveur envoie à son tour un segment `FIN` pour fermer sa propre session d'envoi.
    
      
    
- **Étape 4 — ACK (Client $\rightarrow$ Serveur) :** 
	- Le client confirme la réception par un dernier segment `ACK`. La connexion est totalement fermée.
    
      
    
### 6.4. Champ des bits de contrôle (Flags TCP — 6 bits)

Les indicateurs de contrôle gèrent l'état et la progression de la session TCP :  

- **URG (1 bit) :** 
	- Champ de pointeur urgent significatif (_Urgent Pointer_).
          
    
- **ACK (1 bit) :** 
	- Indicateur d'accusé de réception utilisé lors de l'établissement, du suivi et de la fermeture de la session.
        
    
- **PSH (1 bit) :** 
	- Fonction Push (demande de pousser immédiatement les données vers l'application sans attendre le remplissage des tampons).
    
      
    
- **RST (1 bit) :** 
	- Réinitialisation de la connexion en cas d'erreur ou de dépassement de délai.
    
      
    
- **SYN (1 bit) :** 
	- Synchronisation des numéros de séquence pour l'établissement de la connexion.
    
    
- **FIN (1 bit) :** Plus de données de l'expéditeur, utilisé pour clore la session.


## 7. Fiabilité et contrôle des flux TCP

### 7.1. Fiabilité du TCP — Livraison garantie et commandée

* **Numérotation et réordonnancement :**
	* TCP affecte des numéros d'ordre (numéros de séquence — `SEQ`) à l'en-tête de chaque paquet pour réassembler les segments dans l'ordre original à la destination
	
	* Permet de gérer le réordonnancement des segments si ces derniers ont emprunté des routes différentes et sont arrivés dans le désordre. 


* **Numéro d'ordre initial (ISN — *Initial Sequence Number*) :**
	* Défini de manière aléatoire lors de la création de la session pour renforcer la sécurité contre certaines attaques malveillantes.
	* Incrémenté du nombre d'octets de données transmis au fur et à mesure de la session.


* **Gestion des pertes et retransmission :**
	* **Accusé de réception (ACK) :** Le numéro `ACK` renvoyé par la source indique le premier octet de données que le destinataire s'attend à recevoir ensuite (accusé de réception prévisionnel).
	* **Reconnaissance sélective (SACK — *Selective Acknowledgment*) :** Fonctionnalité optionnelle négociée lors du handshake permettant au récepteur de confirmer explicitement les segments reçus, même s'ils sont discontinus.
	* **Avantage de SACK :** L'émetteur ne retransmet stricto sensu que les segments manquants (ex. seulement les segments 3 et 4 au lieu de réexpédier les segments 3 à 10).
	* **Temporisateurs (*Timers*) :** TCP utilise des minuteries d'attente pour déclencher automatiquement la retransmission d'un segment si aucun `ACK` n'est reçu avant l'expiration du délai.


### 7.2. Contrôle de flux TCP — Taille de fenêtre et MSS

* **Taille de fenêtre (*Window Size*) :**
	* Champ de 16 bits définissant le volume d'octets qu'un équipement peut recevoir et traiter en une seule fois avant d'exiger un accusé de réception.
	* Permet d'ajuster le débit d'envoi pour éviter la saturation du tampon de mémoire du récepteur.

* **Fenêtres coulissantes (*Sliding Windows*) :**
	* Le destinataire transmet des accusés de réception réguliers (`ACK`) qui mettent à jour continuellement la fenêtre d'envoi autorisée.
	* Permet à l'expéditeur de transmettre des données en continu sans interrompre le flux à chaque segment.

* **Taille maximale du segment (MSS — *Maximum Segment Size*) :**
	* Définit la plus grande quantité de données applicatives (en octets) qu'un équipement peut recevoir dans un unique segment TCP.
	* Négociée lors de la poignée de main en trois étapes (`SYN`).
	* Exclut les en-têtes TCP et IP.
	* **Calcul standard (IPv4 sur Ethernet) :**
		* $\text{MTU Ethernet standard} = 1500\text{ octets}$
		* $\text{MSS} = \text{MTU} - (\text{En-tête IPv4} + \text{En-tête TCP}) = 1500 - (20 + 20) = 1460\text{ octets}$



### 7.3. Contrôle d'encombrement TCP

* **Détection de la congestion :**
	* La congestion du réseau provoque des pertes de paquets au niveau des routeurs surchargés.
	* Si des paquets ne parviennent pas à destination ou ne sont pas acquittés à temps, la source interprète ce délai/perte comme un encombrement réseau.

* **Réduction du débit par la source :**
	* Pour éviter de faire empirer la saturation du réseau en retransmettant agressivement, l'émetteur réduit lui-même le nombre d'octets envoyés avant la réception d'un `ACK`.
	* Utilise des algorithmes et minuteries de gestion de congestion pour réguler dynamiquement la vitesse de transmission jusqu'au rétablissement d'un flux fluide.

## 8. Communication UDP

### 8.1. Faible surcharge et fiabilité du protocole UDP

- **Absence de connexion (_Connectionless_) :**
          
    - UDP n'établit aucune connexion préalable (_handshake_) avant d'envoyer des données.
        
    - Fournit un transport de données à faible surcharge grâce à la petite taille de son en-tête (8 octets).
        
    - N'offre aucun mécanisme de gestion du trafic ni de contrôle de flux.
          
        
- **Applications cibles :**
      
    - Adapté aux communications exigeant une vitesse maximale et une faible latence (ex. VoIP, streaming vidéo).
        
          


### 8.2. Réassemblage de datagrammes UDP

- **Gestion des paires et des routes :**    
      
    - Différents datagrammes d'un même flux peuvent emprunter des routes distinctes et arriver dans le désordre au destinataire.    
        
    - UDP n'effectue aucun suivi par numéros d'ordre (contrairement à TCP).
          
        
- **Traitement à la réception :**    
      
    - Les datagrammes sont réassemblés dans l'ordre exact où ils sont reçus et immédiatement transmis à l'application.       
        
    - Aucune retransmission n'est effectuée pour les datagrammes perdus.
          
        
    - Si l'ordre des données est critique, la remise en ordre doit être gérée directement par l'application.
          
        

### 8.3. Processus et requêtes des serveurs UDP

- **Écoute sur les ports serveurs :**    
      
    - Des numéros de ports connus ou inscrits sont affectés aux services UDP (ex. Port `53` pour DNS, Port `1812` pour RADIUS).
        
    - Les applications serveur écoutent sur ces ports et traitent les datagrammes dès leur 
          
        
- **Orientation des flux :**
      
    - UDP transmet les données applicatives au bon processus en fonction du port de destination spécifié dans l'en-tête.
          
### 8.4. Processus des clients UDP

- **Sélection des ports par le client :**
      
    - **Requête client :**
          
        - **Port source :** Sélectionné dynamiquement / aléatoirement par le système client dans la plage des ports éphémères (ex. Port `49152` ou `51152`).
            
        - **Port de destination :** Port réservé du service cible (ex. Port `53` pour DNS ou Port `1812` pour RADIUS).
            
- **Réponse du serveur :**
      
    - Dans sa réponse, le serveur inverse les numéros de ports de la requête initiale :
          
        - **Port source =** Port de destination d'origine du service (ex. Port `53` ou `1812`).
            
        - **Port de destination =** Port source dynamique généré par le client (ex. Port `49152` ou `51152`).