# Modèles et protocoles
#osi #tcp/ip

## Définitions 
#Protocole : ensembles de règles convenues qui ont été créés par les organismes de normalisation;

#Modèles  permettent de visualiser les règles et leur place dans votre réseau

Pourquoi : 
- Permet de comprendre la façon dont les réseau fonctionnent réellement.
- Visualiser les échange de paquets; Le codage formats et organisation des messages échangés

Dans le quotidien on utilise des protocoles Méthodes de communication 
	- entrée dans une classe 
		- Toquer -> attendre réponse -> réponse reçue -> saluer ->
	- RDV avec le roi
		- Heure - tenue. vestimentaire - Présentation - Attendre que le Roi de tendre la main - l'appeler Sir quand tu t'adresse à lui - l'appeler Sa Majesté quand tu parle de lui
	- Appel téléphonique 
		- Allo - répéter si pas de réponse - Se présenter - Commencer à parler
	- La poste 
		- Courier - adresse de destination - adresse de l'expéditeur - lieu de dépôt - Si colis emballage spécial - Recommandé  ou non ?

## Les Règles 


### Communication 
Les Protocoles de communication respectent des règles souvent selon ce schémas 

![[Screenshot 2026-09-11 at 16.35.19.png]]

- L'identification de l'expéditeur et du destinataire ;
- L'utilisation d'une langue et d'une syntaxe communes ;
- la vitesse et le rythme d'élocution ;
- la demande de confirmation ou d'accusé de réception.
### Exigences des Protocoles Réseau 

- Codage des messages
	- Conversion vers un format acceptable 
	- Implique le décodage à la destination
- Format et encapsulation des messages 
	- Structure spécifique 
		- Modèle OSI
	- Dépend du type de message et du type de canal utilisé
- La taille du message
	- message découpé en plusieurs partie des #Trames (contenant chacune un fragment du message initial)
	- Différent selon le canal 
	- reconstruction à la destination 
- Synchronisation des messages
- Options de remise des messages
#### Exemple d'un paquet IP
- Paquet IP
	- ![[Screenshot 2026-09-11 at 16.51.55.png]]
#### Mode de transmission du message 
- option de livraison 
	- ![[Pasted image 20260922144939.png]]




## Protocoles 
Respecter un même ensemble de Règles 
==Protocoles réseau==  définit un format, un ensemble de règles d'échanges de messages entre périphériques réseaux 

### Plusieurs Types
- **Réseaux** : échange entre périphériques réseau 
	- IP 
		- Internet Protocol (IPv4/IPv6)
	- TCP
		- Transmission Control Protocol
	- HTTP
		- HyperText Transfer Protocol
- **Sécurité  Réseaux** : assurer authentification, intégrité  et le cryptage des données
	- SSH
		- Secure Shell => connexion distant à un Systeme 
	- SSL
		- Secure Sockets Layer => chiffrer la communication entre  client / serveur (ancien) .
	- TLS
		- Transport Layer Security => remplace SSL, chiffrer les données transitant sur un réseau (HTTPS)
- **Routage** : informations de routage
	- OSPF
		- Open Shortest Path First 
	- BGP
		- Border Gateway Protocol
- **Découverte de Services**
	- DHCP
		- Dynamic Host Configuration Protocol => attribution automatique d'adresses IP
	- DNS
		- Domain Name Sever => traduction de noms(URL) en adresses IP
			- `ping google.be`

### Interaction entre Protocoles 
Utilisation de plusieurs protocoles
- Exemple : envoie d'un mail  
	- ![[Pasted image 20260922151920.png]]
	- **_Ethernet_**
		- remise des messages d'une carte réseau à une autre carte réseau sur le même réseau local (LAN)
	- **_IP(Protocole Internet )_** 
		-  remise des messages de l'expéditeur au destinataire. IP utilisé par les routeurs traverser plusieurs réseaux.
	- **_TCP(Transmission Control Protocol )_**
		- garantir la livraison fiable des informations et de gérer le contrôle du flux.
	- **_HTTP(HyperText Transfer Protocol )_**
		- interaction entre serveur web et  client.

## Suite de Protocoles 
Groupe de protocoles interdépendants pour assurer une fonction de communication 
chaque service de niveau supérieur dépend des fonctionnalités du niveau inférieur 
- => couches 
	- ![[Pasted image 20260922155604.png]]
	- **_Internet Protocol Suite ou TCP/IP_** 
		- Protocoles standard ouverte gérée par Internet Engineering Task Force (IETF).
	- **_Protocoles d'interconnexion de systèmes ouverts (OSI)_** -  
		- élaborés conjointement e l'Organisation internationale de normalisation (ISO) et l'Union internationale des télécommunications (UIT). 
		- Modèle de référence OSI en è couches .  Les protocoles OSI ont été largement remplacés par TCP/IP.
			- Diagramme du modèle OSI![[Pasted image 20260922160202.png]]
				- ![[Pasted image 20260922160507.png]]
				- ![[Pasted image 20261005210155.png]]
## Organisme de normalisation 
- **_ISOC (Internet Society)_**
	- _Responsable de la promotion du développement ouvert et de l'évolution de l'utilisation de l'internet dans le monde entier._
- **_IAB (Internet Architecture Board)_** 
	- _Responsable de la gestion globale et du développement des normes Internet._
- **_IETF (Internet Engineering Task Force)_** 
	- Développe, met à jour et assure la maintenance des technologies Internet et TCP/IP. Cela inclut le processus et les documents pour le développement de nouveaux protocoles et la mise à jour des protocoles existants, qui sont connus sous le nom de documents de demande de commentaires (RFC).
	- RFC 791 (IP - Internet Protocol)
		- Définit les règles de base du protocole IPv4, la structuration des paquets de données et le principe d'adressage IP.
		- https://www.rfc-editor.org/info/rfc791/
	- **RFC 793 (TCP - Transmission Control Protocol)
		- Spécifie le fonctionnement de TCP, assurant un transport de données fiable et orienté connexion.
		- https://www.rfc-editor.org/info/rfc793/
	- **RFC 1034 et 1035 (DNS)
		- Définissent le fonctionnement du système de noms de domaine, qui traduit les noms de domaine en adresses IP.
		- https://www.rfc-editor.org/info/rfc1034/
		- https://www.rfc-editor.org/info/rfc1035/
- **_IRTF (Internet Research Task Force)_**
	- _Se concentre sur la recherche à long terme liée à l'internet et aux protocoles TCP/IP comme l'Anti-Spam Research Group (ASRG), le Crypto Forum Research Group (CFRG) et le Peer-to-Peer Research Group (P2PRG)._
- **_ICANN (Internet Corporation for Assigned Names and Numbers)_**- 
	- _Basée aux États-Unis, l'ICANN coordonne l'attribution des adresses IP, la gestion des noms de domaine et l'attribution d'autres informations utilisées dans les protocoles TCP/IP._
- **_IANA (Internet Assigned Numbers Authority)_**- 
	- _Responsable de la supervision et de la gestion de l'attribution des adresses IP, de la gestion des noms de domaine et des identificateurs de protocole pour l'ICANN._
	- département de l'ICANN chargé de l'exécution au quotidien des tâches techniques.
- **IEEE (Institute of Electrical and Electronics Engineers)(IEEE, (prononcé "I-triple-E")**
	- Crée des normes électriques et informatiques fondamentales pour les réseaux, comme Ethernet (802.3) et le Wi-Fi (802.11).
		- https://standards.ieee.org/ieee/802.3/10422/
		- https://standards.ieee.org/ieee/802.11/7028/
		- https://standards.ieee.org/ieee/1588/4355/
    
- **EIA (Electronic Industries Alliance)**
	- Normalise les équipements physiques de réseau, notamment le câblage électrique, les connecteurs et les baies de brassage (racks 19 pouces)
    
- **TIA (Telecommunications Industry Association)**
	- Définit les normes pour les infrastructures de télécommunications (voix sur IP, antennes relais, satellites) et co-développe les standards de câblage réseau avec l'EIA.
    
- **UIT-T (Union internationale des télécommunications - Secteur de la normalisation des télécommunications)**
	- Établit des normes internationales pour les technologies de communication globale, telles que la compression vidéo, l'IPTV et le haut débit DSL

## Encapsulation
- Notion d'encapsulation 
	- ![[Pasted image 20260922160800.png]]
		- ![[Pasted image 20260922164212.png]]
- 
- **Données** : 
	- terme générique pour l'unité de données de protocole utilisée à la couche application
- **Segment** : 
	- unité de données de protocole de la couche transport
- **Paquet** : 
	- unité de données de protocole de la couche réseau
- **Trame** : 
	- Couche Liaison de Données PDU
- **Bits** : 
	- Couche Physique PDU utilisée lors de la transmission physique de données sur le support

> [!Remarque] 
>  Si l'en-tête Transport est TCP, il s'agit d'un **segment**. Si l'en-tête Transport est UDP, il s'agit d'un **datagramme**..
>


## TP-PT

![[3.5.5-packet-tracer---investigate-the-tcp-ip-and-osi-models-in-action_fr-FR.pdf_fr-FR_985dd510-ac11-4034-ba9f-951dc1d27a6b(1).pdf]]







