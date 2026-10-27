

# Module 15 Couche Application
## 1 Application, Présentation et Session

### 1.1. Correspondance Modèle OSI vs Modèle TCP/IP

Dans le modèle **TCP/IP**, la couche Application regroupe les fonctions des **trois couches supérieures du modèle OSI** (Application, Présentation, Session).

```text
  Modèle OSI             Modèle TCP/IP
+-------------------+   +-------------------+
| 7. Application    |   |                   |
+-------------------+   |                   |
| 6. Présentation   |  -->  Application     |
+-------------------+   |                   |
| 5. Session        |   |                   |
+-------------------+   +-------------------+

```



### 1.2. Rôles des couches supérieures OSI

* **Couche 7 - Application :** 
	* Sert d'interface directe entre les programmes utilisateurs et le réseau sous-jacent.
* **Couche 6 - Présentation :**
	* **Mise en forme / Formats :** Formate et présente les données (MKV, MOV, MPEG, GIF, JPG, PNG).
	* **Compression :** Compresse les données pour le transfert.
	* **Chiffrement :** Chiffre les données à l'émission / déchiffre à la réception.
	
* **Couche 5 - Session :** 
	* Crée, gère, maintient et ferme les dialogues/sessions entre les applications source et destination.



### 1.3. Cheat-Sheet des Protocoles applicatifs TCP/IP 

> [!danger] **à retenir (CCNA 1)**
>

| Catégorie | Protocole | Rôle / Description | Port(s) & Transport |
| --- | --- | --- | --- |
| **Résolution de nom** | **DNS** | Traduit les noms de domaine (ex: `cisco.com`) en adresses IP. | **UDP / TCP 53** |
| **Configuration d'hôte** | **DHCP** | Attribue dynamiquement des adresses IP et options réseau aux hôtes. | **Client UDP 68 / Serveur UDP 67** |
|  | **BOOTP** | Ancien protocole (remplacé par DHCP) pour démarrer des stations sans disque. | **Client UDP 68 / Serveur UDP 67** |
| **Messagerie (E-mail)** | **SMTP** | Envoie des e-mails d'un client vers un serveur ou entre serveurs. | **TCP 25** |
|  | **POP3** | Télécharge les e-mails du serveur vers le client local (suppression/gestion locale). | **TCP 110** |
|  | **IMAP** | Accède et maintient les e-mails directement sur le serveur de messagerie. | **TCP 143** |
| **Transfert de fichiers** | **FTP** | Transfert de fichiers fiable orienté connexion (avec accusé de réception). | **TCP 20** (Données) & **TCP 21** (Contrôle) |
|  | **TFTP** | Transfert de fichiers léger, non connecté (UDP) et sans accusé de réception. | **UDP 69** |
| **Web** | **HTTP** | Échange de contenu web (texte, images, vidéo). | **TCP 80 / 8080** |
|  | **HTTPS** | HTTP sécurisé par chiffrement et authentification du site. | **TCP / UDP 443** |




## 2 Modèle Client-Serveur et Peer-to-Peer (P2P)

### 2.1. Modèle Client-Serveur

* **Définition :** 
	* **Client :** Équipement/application qui initie la requête d'information. 
	* **Serveur :** Équipement/application qui stocke les ressources et répond aux requêtes des clients.
* **Flux de données :** 
	* **Téléchargement descendant (*Download*) :** Transfert de données du serveur vers le client.
	* **Téléchargement ascendant (*Upload*) :** Transfert de données du client vers le serveur.
* **Exemple :** Services de messagerie (SMTP/POP3/IMAP), serveurs web (HTTP/HTTPS), serveurs de fichiers.

### 2.2. Réseaux Peer-to-Peer 

* **Principe :** 
	* Deux hôtes ou plus sont connectés et partagent des ressources (fichiers, imprimantes, connexion Internet) **sans serveur dédié**.
* **Rôles symétriques :** 
	* Chaque hôte (*peer* / homologue) peut agir **simultanément comme client et comme serveur** selon le sens de la transaction.
* **Fonctionnement :** 
	* Le rôle est déterminé par requête.
	* *Exemple :* L'homologue 1 partage un fichier avec l'homologue 2, tout en utilisant l'imprimante physiquement raccordée à l'homologue 2.



### 2.3. Applications P2P et Réseaux hybrides 

* **Applications P2P :** 
	* Nécessitent une interface utilisateur et un service en arrière-plan s'exécutant sur chaque appareil (ex: messagerie instantanée, voix/visio).
* **Systèmes Hybrides :** 
	* Le partage de ressources est décentralisé entre les pairs, mais un **serveur d'index centralisé** conserve l'annuaire indiquant où se trouvent les ressources.

#### Protocoles et Réseaux P2P courants :
* **Gnutella :** Protocole P2P permettant de rechercher et partager des fichiers sur le réseau (clients : BitComet, μTorrent, Deluge, eMule).
* **BitTorrent :** 
	* Utilise de petits fichiers métadonnées (**`.torrent`**) pour localiser d'autres utilisateurs possédant des morceaux du fichier recherché.
	* Permet le téléchargement simultané de parties de fichiers auprès de multiples pairs à la fois.
* **Autres réseaux :** Direct Connect, eDonkey, Freenet.

> [!WARNING] Aspect Légal
> Le protocole P2P en soi est une technologie neutre. Cependant, le partage ou le téléchargement de fichiers protégés par le droit d'auteur sans l'autorisation des ayants droit est illégal et passible de sanctions pénales.



## 3 Protocoles web et messagerie électronique

### 3.1. Web : HTTP, HTTPS et HTML

Lorsqu'un utilisateur saisit une URL (ex: `http://www.cisco.com/index.html`), le navigateur la décompose :
* **Protocole (schéma) :** `http` (détermine le protocole d'application).
* **Nom du serveur :** `www.cisco.com` (traduit en IP via DNS).
* **Nom du fichier :** `index.html` (page demandée).

#### Commandes/Requêtes HTTP courantes :
* **GET :** Requête du client pour récupérer des données (ex: charger une page HTML).
* **POST :** Envoi de données vers le serveur web (ex: formulaires, données d'identification).
* **PUT :** Téléversement de ressources ou contenu vers le serveur web (ex: envoi d'une image).

#### Sécurisation (HTTP vs HTTPS) :
* **HTTP (TCP 80) :** Données transmises en texte clair (non sécurisé, vulnérable à l'interception).
* **HTTPS (TCP 443) :** Utilise le chiffrement et l'authentification via **SSL/TLS** pour sécuriser les échanges entre le client et le serveur.


### 3.2. Services de Messagerie Électronique

Les clients de messagerie ne communiquent jamais directement entre eux ; ils s'appuient sur des **serveurs de messagerie**.

```text
[Expéditeur] ---> (SMTP) ---> [Serveur FAI A] ---> (SMTP) ---> [Serveur FAI B] ---> (POP3/IMAP) ---> [Destinataire]
````

#### Les 3 Protocoles de Messagerie :

|**Protocole**|**Port / Transport**|**Rôle principal**|
|---|---|---|
|**SMTP** _(Simple Mail Transfer Protocol)_|**TCP 25**|**Envoi d'e-mails :** Utilisé par le client pour envoyer un mail au serveur, et entre serveurs de messagerie.|
|**POP3** _(Post Office Protocol)_|**TCP 110**|**Téléchargement d'e-mails :** Récupère les messages du serveur et les télécharge sur l'appareil local (les messages sont généralement supprimés du serveur).|
|**IMAP** _(Internet Message Access Protocol)_|**TCP 143**|**Accès et synchronisation :** Maintient et synchronise les e-mails directement sur le serveur. Idéal pour consulter ses mails sur plusieurs appareils.|


## 4 Services d'adressage IP (DNS et DHCP)

### 4.1. DNS - Domain Name System

* **Rôle :** 
	* Traduit un nom de domaine (FQDN, ex: `www.cisco.com`) en adresse IP numérique.
* **Format & Cache local :**
	* *Windows :** `ipconfig /displaydns` affiche le cache DNS local du PC.
	* *Outil de diagnostic :** `nslookup` permet de tester et interroger manuellement un serveur DNS.

#### Principaux enregistrements DNS (Resource Records) :

| Type | Description / Rôle |
| :---: | :--- |
| **A** | Associe un nom de domaine à une adresse **IPv4** (32 bits)[cite: 56]. |
| **AAAA** | Associe un nom de domaine à une adresse **IPv6** (128 bits, prononcé *quad-A*)[cite: 56]. |
| **NS** | Désigne un serveur de noms faisant autorité pour la zone (*Name Server*)[cite: 56]. |
| **MX** | Oriente le courrier électronique vers le serveur de messagerie (*Mail Exchange*)[cite: 56]. |

#### Hiérarchie DNS :
* **Domaine racine (`.`) :** Sommet de la hiérarchie DNS.
* **Domaines de premier niveau (TLD) :** `.com`, `.org`, `.net`, `.edu`, extensions nationales (`.fr`, `.be`, `.ca`).
* **Domaines de second niveau :** `cisco.com`, `google.com.



### 4.2. DHCP - Dynamic Host Configuration Protocol

* **Rôle :** 
	* Automatise l'attribution dynamique des adresses IP, masques, passerelles et serveurs DNS aux hôtes.
* **Durée de bail (*Lease*) :** Durée limitée pendant laquelle l'IP est attribuée au client. À l'expiration, l'adresse doit être renouvelée ou renvoyée au pool.
* **Bases d'adressage :**
  * **Statique :** Pour les serveurs, imprimantes, commutateurs et interfaces de routeurs.
  * **Dynamique :** Pour les équipements utilisateurs (PC, téléphones, PC portables).



### 3. Le Processus DORA (DHCPv4)

Le dialogue client-serveur DHCPv4 se déroule en **4 étapes clés** :

```text
 Client DHCP                                           Serveur DHCP
     |                                                       |
     | ------------ 1. DHCPDISCOVER (Broadcast) -----------> |
     | <----------- 2. DHCPOFFER (Unicast/Broadcast) ------- |
     | ------------ 3. DHCPREQUEST (Broadcast) ------------> |
     | <----------- 4. DHCPACK (Unicast/Broadcast) --------- |
````

1. **`D`ISCOVER (Client $\to$ Serveur) :** 
	- Le client recherche un serveur DHCP disponible (Diffusion / Broadcast).
        
2. **`O`FFER (Serveur $\to$ Client) :** 
	- Le serveur propose un bail (IP, masque, passerelle, DNS).
    
3. **`R`EQUEST (Client $\to$ Serveur) :** 
	- Le client accepte officiellement l'offre et informe le réseau (Broadcast).
       
4. **`A`CK (Serveur $\to$ Client) :** Le serveur confirme le bail et enregistre l'attribution.
    
      
    

> [!NOTE] Équivalents pour DHCPv6 Les messages DHCPv6 équivalents au processus DORA sont : **SOLICIT**, **ADVERTISE**, **INFORMATION REQUEST** et **REPLY**.


## 5 Services de partage de fichiers

### 5.1. Protocole FTP - File Transfer Protocol

* **Rôle :** 
	* Protocole client/serveur permettant de télécharger (*download*) ou de verser (*upload*) des fichiers entre un client et un serveur FTP[.
* **Architecture à double connexion :**
	* **Connexion de contrôle (Port TCP 21) :** Établie en premier pour l'authentification et l'envoi des commandes client / réponses serveur.
	* **Connexion de données (Port TCP 20) :** Créée à chaque transfert effectif de fichier (dans un sens ou dans l'autre)

### 5.2. Protocole SMB - Server Message Block

* **Rôle :** 
	* Protocole client/serveur de requête-réponse servant au partage de ressources réseau (fichiers, répertoires, imprimantes, ports série).
* **Fonctions clés :**
	  1. Démarrer, authentifier et terminer les sessions.
	  2. Contrôler l'accès aux fichiers et aux imprimantes.
	  3. Acheminer des messages vers d'autres équipements.
* **Caractéristiques clés :**
	* **Connexion à long terme :** Contrairement à FTP qui ouvre une connexion par transfert, SMB maintient une connexion prolongée. Les ressources distantes apparaissent comme si elles étaient locales sur le client.
	* **Intégration Microsoft & Interopérabilité :** Base des réseaux Windows. Pris en charge sur Linux/UNIX via **SAMBA** et nativement sur macOS.