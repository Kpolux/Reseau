#ios 
# Configuration de base Switch


## OS (Operating System / Système d'Exploitation - SE)
### 1. Structure d'un Système d'Exploitation (OS)

- **Noyau (Kernel) :** 
	- fait le lien direct entre le matériel et le logiciel.
- **Interprète de commandes (Shell) :**
	- interface exécutant les requêtes utilisateur.
- **Matériel (Hardware) :**
	- composants électroniques physiques.

- Exemples OS : 
	- PC : Windows - macOS - Linux (Plusieurs distribution) 
	- Smartphone : Apple iOS - Android 

### 2. Modes d'accès (Interfaces)

- CLI (_Command line interface_ / Interface de ligne de commande)
	- Environnement textuel (invite de commande)
		- ![[Pasted image 20260923111455.png]]
	- Nécessité de connaitre structure de commande et les commandes 
	- **Avantages réseau :** très légère (faible surcharge), plus stable, donne accès à 100% des fonctions.
- GUI (_Graphical User Interface_ / Interface utilisateur graphique)
	- Environnement visuel (icônes, menus, fenêtres).
	- Intuitif, convivial, sans connaissances préalables.
	- **Limites :** fonctionnalités parfois restreintes, risque de pannes/bugs.
	
### 3. Objectif d'un OS

- **Interaction système :** 
	- Permet à un utilisateur (via GUI) ou à un technicien (via CLI sur Cisco IOS) d'exécuter des commandes et d'interagir avec le matériel réseau.
- **Versions et mises à niveau :** 
	- Chaque équipement exécutant Cisco IOS dispose d'une version spécifique selon son modèle et ses besoins applicatifs.
- **Évolutivité :** 
	- Il est possible de mettre à jour la version de l'IOS ou l'ensemble de ses fonctionnalités pour obtenir de nouvelles options de configuration.
### 4. Spécificités Cisco

- **Cisco IOS (Internetwork Operating System/Système d'exploitation interréseau ) :** 
	- OS principal des routeurs/switchs Cisco.
- **Autres versions :** 
	- IOS XE, IOS XR, NX-OS.

> [!Note] 
>  - **Routeurs grand public :**  utilisent un _firmware_ généralement configuré via GUI Web.
>  - Certain marque utilise un OS inspiré de IOS exemple *ARISTA* utilise EOS(définition )
>
 

### 5. Méthodes d'accès

- **OBB (Out-of-Band / Hors Bande) :**
    
    - **Console :** 
	    - Connexion physique directe via un câble console et un **logiciel d'émulation de terminal**. Accessible sans adresse IP ni service réseau actif (configuration initiale /Maintenance).
        
    - **AUX (Auxiliaire) :**
	    - Ancien port d'accès hors bande utilisant une ligne téléphonique/modem, sans nécessiter de réseau actif.
        
- **IB (In-Band / En Bande) :**
    
    - **SSH (Secure Shell) :** 
	    - Connexion CLI à distance **sécurisée et chiffrée** traversant le réseau de données. Nécessite une adresse IP et des services réseau actifs.
        
    - **Telnet :** 
	    - Connexion CLI à distance **non sécurisée** (données et mots de passe transmis en clair). À proscrire en production au profit de SSH.

> [!Remarque ] 
>  - **Un switch:**  n'a pas forcement besoin de configuration initiale pour fonctionner. par défaut il transmet le trafic.
>  - **Un logiciel d'émulation de terminal** peut être utiliser pour une connexion SSH ou Telnet 
>



- **Programme d'émulation de terminal** 
	- **Tera Term**
		- ![[Pasted image 20260923120804.png|451]]
	- **Putty**
		- ![[Pasted image 20260923120845.png]]
- **Invite de commande de votre OS Préféré** 
	- Syntaxe de base
		- `ssh utilisateur@adresse_ip`
	- Connexion standard (port 22 par défaut)
		- `ssh admin@192.168.1.1`
	- Connexion en spécifiant un port particulier (ex: port 2222) 
		- `ssh -p 2222 admin@192.168.1.1`






## IOS (Internetwork Operating System/Système d'exploitation interréseau )
Configuration en CLI

### 1. Deux Modes  Principaux

- **Mode d'exécution Utilisateur (*User EXEC Mode*) :**
    
    - **Invite de commande :** 
	    - `Switch>` ou `Router>`
        
    - **Rôle :** 
	    - Surveillance de base uniquement (_view-only_).
	    
	- **Accès :** 
		- Par défaut
        
    - **Caractéristique :** 
	    - Aucune modification de configuration n'est autorisée.
        
- **Mode d'exécution Privilégié (*Privileged EXEC Mode* / Mode Enable) :**
    
    - **Invite de commande :** 
	    - `Switch#` ou `Router#`
	    
	- **Accès :** 
		- Commande `enable` depuis l'invite initiale
		- Commande `disable` depuis le mode privilège (`#`)
        
    - **Rôle :** 
	    - Accès complet à toutes les commandes de surveillance, de diagnostic et de gestion.
        
    - **Caractéristique :** 
	    - Passage obligatoire pour accéder aux modes de configuration supérieurs.

###  2. Modes  de configuration et sous-modes

Il existe de nombreux sous-modes accessibles 

- ***Configuration Globale* :**
    souvent appeler le mode ***actif*** 
    
    - **Invite  de commande :** 
	    - `Switch(config)#`
	    
	- **Accès :** 
		- Commande `configure terminal`. **depuis le mode mode privilège (`#`)**.
        
    - **Rôle :** 
	    - Permet d'appliquer des modifications affectant l'ensemble du périphérique.
	    - Passage obligatoire pour accéder aux sous-modes spécifiques.
        
- **Sous-modes de configuration courants :**
	
	Il existe de nombreux sous-modes accessibles **depuis le mode mode Configuration Globale (config)#`.
	
	- Exemples : 
	    - **Ligne (`line`) :**
	        - **Invite  de commande :** 
		        - `Switch(config-line)#`
	        
		    - **Accès :** 
			    - **`line`** *Type-ligne* *Numéro-ligne* *\[FinRange-ligne]*
			    - Ex. `line console 0` ou `line vty 0 15`.
            
	        - **Rôle :** 
		        - Configuration des accès de gestion (Console, SSH, Telnet, AUX).
            
	    - **Interface (`interface`) :**
	        - **Invite  de commande :** 
		        - `Switch(config-if)#`
	        
		    - **Accès :** 
			    - **`interface`** *interface-id*
			    - Ex. `interface gigabitethernet 0/1`
            
	        - **Rôle :** 
		        - Configuration des ports réseau et cartes d'interface du Switch ou du routeur.
		        
		        
		        
### 3. Commandes de navigation de base
- **Reculer d'un niveau :** `exit`
	- Passe au mode immédiatement supérieur dans la hiérarchie.
		- Ex. : `Switch(config-if)# exit` $\rightarrow$ `Switch(config)#`
		- `exit` depuis mode privilège (`#`) fais sortir du terminal 
	
- **Retour direct au Mode Privilège (`#`) :** `end` ou `Ctrl+Z`
	- Quitte tous les sous-modes de configuration d'un seul coup.
		- Ex. : `Switch(config-if)# end` $\rightarrow$ `Switch#`
        
- **Saut direct entre sous-modes :** saisir directement la commande du sous-mode cible
	- Permet de changer de contexte sans repasser par le mode de configuration globale.
		- Ex. : `Switch(config-line)# interface vlan 1` $\rightarrow$ `Switch(config-if)#`
		- Ex. : `Switch(config-if)# gigabitethernet 0/1` $\rightarrow$ `Switch(config-if)#`

### 4. Syntaxe et AIDE dans Cisco IOS

- **Structure d'une commande**
	
	- ![[Pasted image 20260923142153.png]]
	
	- **Invite (Prompt) :**
		- indique le mode de commande actuel
		- ex. `Switch>`.
    
	- **Commande :** 
		- l'action principale à exécuter 
		- ex. `show`, `ping`
    
	- **Espace :** 
		- séparateur obligatoire entre la commande, les mots-clés et les arguments.
    
	- **Mot-clé (Keyword) :** 
		- paramètre prédéfini dans l'OS (ex. `ip protocols`).
    
	- **Argument :** 
		- valeur ou variable définie par l'utilisateur (ex. adresse IP `192.168.10.5`).
	
- **Aide IOS**

	- **Aide contextuelle (`?`) :**
    
	    - `?` seul : 
		    - affiche toutes les commandes disponibles dans le mode actuel.
        
	    - `ch?` : 
		    - affiche toutes les commandes commençant par "ch".
        
	    - `show ?` : 
		    - affiche les mots-clés/arguments disponibles après la commande `show`.
        
	- **Vérification de syntaxe :** 
		- L'interprète de commandes analyse la saisie de gauche à droite et retourne un message d'erreur explicite en cas de commande incomplète, inconnue ou ambiguë.
		
	- Abréviation des commandes
		- Les commandes et mots-clés peuvent être raccourcis au **nombre minimal de caractères uniques**.
			- _Exemple :_ `conf` pour `configure` (mais `con` ne fonctionne pas car ambigu).
			- `confugure Terminal`  $\rightarrow$ `conf t` 
			- `enable`  $\rightarrow$ `en` 
			- `interface gigabitethernet 0/1` $\rightarrow$ `int g0/1`
	- Raccourcis 
		- **TAB - Tabulation** : 
			- **Complétion**: Complète automatiquement une commande ou un mot-clé partiellement saisi.
		- Flèche Haut / Ctrl + P :
			- Rappelle les commandes précédentes (du plus récent au plus ancien).
		- **Ctrl + C**
			- Sort du mode de configuration vers le mode d'exécution privilégié (ou réaffiche l'invite).
		- Ctrl + Z
			- [[#3. Commandes de navigation de base]]
		- **Ctrl + A** / **Ctrl + E**
			- Déplace le curseur à la **Début(Ahead) /fin(End) de la ligne**.
		- **Ctrl + Shift + 6**
			- **Séquence d'interruption globale :** annule l'exécution en cours (ping, traceroute, recherche DNS).


### 5. Config de Base
#### 1. Nom de périphérique (Hostname)

- **Utilité :**
	Identifie l'équipement lors des accès à distance (ex: SSH) et dans la documentation.
    
- **Règles de nommage :**
	Débute par une lettre, < 64 caractères, sans espace, contient uniquement lettres, chiffres et tirets.
    
- **Commandes :**
    
    - `Switch(config)#` **`hostname`** _Nom-du-Switch_
        
    - `Switch(config)#` **`no hostname`** _(Restaure le nom par défaut)_
        
#### 2. Sécurisation des accès (Mots de passe)

> [!Attention] 
>  - Dans les laboratoires et exercices d'apprentissage (NetAcad, Packet Tracer), des mots de passe simples (ex. `cisco` ou `class`) sont souvent utilisés pour faciliter les manipulations.
>  - **À NE JAMAIS REPRODUIRE EN PRODUCTION :** Dans un environnement professionnel, l'utilisation de mots de passe faibles représente un risque majeur pour la sécurité du réseau. Veillez à toujours appliquer une politique de mots de passe forts


- **Mode d'exécution Privilégié (Accès critique) :**
    
    - `Switch(config)#` **`enable secret`** _mot-de-passe_ _(Chiffre le mot de passe de manière forte)_
        
- **Accès Console  :**
    
    - `Switch(config)#` **`line console 0`**
        
    - `Switch(config-line)#` **`password`** _mot-de-passe_
        
    - `Switch(config-line)#` **`login`** _(Active la demande de mot de passe)_
        
- **Accès Lignes Virtuelles / Telnet & SSH (In-Band) :**
    
    - `Switch(config)#` **`line vty 0 15`**
        
    - `Switch(config-line)#` **`password`** _mot-de-passe_
        
    - `Switch(config-line)#` **`login`**
        

#### 3. Chiffrement de tous les mots de passe

- **Commande :** 
	- `Switch(config)#` **`service password-encryption`**
    
- **Rôle :** 
	- Chiffre en texte clair (Type 7) tous les mots de passe non chiffrés apparaissant dans la configuration (`running-config`).
    
- **Vérification :** 
	- `Switch#` **`show running-config`**
    

#### 4. Message de bannière (MOTD - Message Of The Day)

- **Utilité :** 
	- Affiche une notification légale pour avertir que l'accès non autorisé est interdit.
    
- **Commande :** 
	- `Switch(config)#` **`banner motd #`** _message_de_banniere_ **`#`**
    
- **Spécificité :** 
	- Le caractère `#` sert de délimiteur de début et de fin de message.

#### 5. Fichier de config :



##### Script

```cisco
! ===================================================
! CONFIGURATION DE BASE CISCO IOS
! ===================================================

! Passage en mode configuration globale
enable
configure terminal

! 1. Nom de l'équipement
hostname SW-name

! 2. Sécurisation de l'accès au mode Privilégié
enable secret SuperSecretPass123!

! 3. Sécurisation de la ligne Console (Accès physique OOB)
line console 0
 password ConsolePass123!
 login
 exit

! 4. Sécurisation des lignes VTY (Accès réseau à distance 0 à 15)
line vty 0 15
 password RemotePass123!
 login
 exit

! 5. Chiffrement local de tous les mots de passe (Type 7)
service password-encryption

! 6. Message de bannière d'avertissement (MOTD)
banner motd #
===================================================
  ATTENTION: Accès réservé au personnel autorisé!
  Toute tentative d'accès non autorisé sera poursuivie.
===================================================
#

! Fin du mode de configuration
end

! 7. Sauvegarde en mémoire non-volatile (NVRAM)
copy running-config startup-config
```

> [!Note importante] 
>  - ligne commençant par **`!`**, l'IOS l'ignore complètement (considéré comme un commentaire)
>  - à la **fin** d'une commande (ex: `enable secret pass123!`), le `!` sera inclus dans le mot de passe !
>  - L'IOS **ne gère pas** le commentaire au coup par coup dans l'invite. Taper `! mon commentaire` renverra une erreur de commande inconnue.
>  - L'IOS utilise automatiquement le symbole **`!`** comme **séparateur visuel** entre les différents blocs de configuration dans le résultat de (`show running-config`)


##### Exemple 
> [!astuces] 
>  - Cérer des fichier de config pour vos équipement pret à l'emploi
>  - 1 pour les switch 
>  - 1 pour les routeurs 


[[Switch_Template]]
[[Router_Template]]


#### 6. enregistrement de config 
- Fichiers de configuration
	- **`running-config` (RAM) :** 
		- Configuration active en mémoire volatile. Effacée lors d'une extinction ou d'un redémarrage.
    
	- **`startup-config` (NVRAM) :** 
		- Configuration de démarrage enregistrée en mémoire non volatile. Conservée après redémarrage.
- Commandes de consultation
	
	- **Afficher la configuration active :** 
		- `Switch#` **`show running-config`**
    
	- **Afficher la configuration de démarrage :** 
		- `Switch#` **`show startup-config`**
		
- Sauvegarder la configuration

	- **Commande :** 
		- `Switch#` **`copy running-config startup-config`**
    
	- **Rôle :** 
		- Copie les modifications de la RAM vers la NVRAM pour les rendre permanentes.
		
- Voir Mémoire ou dossier de config 
	- `Switch#` **dir** (directory)
	
- Annuler des modifications / Réinitialiser

	- **Annuler les modifications non enregistrées :** 
		- `Switch#` **`reload`**
		    - Redémarre l'équipement pour recharger la `startup-config`. Refusez l'enregistrement si l'IOS le demande (`n` ou `no`).
        
	- **Effacer la configuration d'usine (Reset complet) :**
    
	    1. `Switch#` **`erase startup-config`** (Supprime le fichier en NVRAM).
        
	    2. `Switch#` **`reload`** (Redémarre l'appareil à vide).

#### 7. Adressage IP, Interfaces et Supports

- Notions d'adressage IP
	
	- **IPv4 :** Adresse de 32 bits codée en decimal pointé (ex: `192.168.1.10`).
    
	    - **Masque de sous-réseau :** Délimite la partie réseau et la partie hôte (ex: `255.255.255.0`).
        
	    - **Passerelle par défaut :** Adresse IP du routeur permettant de sortir du réseau local (ex: `192.168.1.1`).
        
	- **IPv6 :** Adresse de 128 bits codée en hexadécimal (ex: `2001:db8:acad:10::10/64`).
    

-  Supports physiques (Médias réseau)
	 Le choix du support dépend de la distance, du débit, de l'environnement et du coût :
	
	- **Cuivre :** 
		- Câbles à paires torsadées (Ethernet).
    
	- **Fibre optique :** 
		- Pour les longues distances et très hauts débits.
    
	- **Sans fil (Wi-Fi) :** 
		- Onde radio pour équipements mobiles.
    

- Interfaces physiques vs SVI (Commutateurs)
	- **Ports physiques (Couche 2) :**
		- Permettent le raccordement physique des équipements. 
		- Sur un switch de couche 2, ils ne supportent pas l'attribution directe d'adresses IP.
		
	- **Interface Virtuelle de Commutateur (SVI) :** 
		- Interface logique (logicielle) dédiée à la gestion à distance du switch (In-Band) via IPv4/IPv6.
	    - **SVI par défaut :** `VLAN 1` est présente nativement sur tous les commutateurs Cisco.
	    
- Configuration de l'adressage IP sur les hôtes et commutateurs
	
	- Adressage IP des périphériques finaux (PC, Serveurs)
		- **Configuration manuelle (Statique) :** L'administrateur saisit manuellement l'adresse IP, le masque de sous-réseau et la passerelle par défaut dans les paramètres de la carte réseau.
		- **Configuration automatique (Dynamique - DHCP) :** Le protocole **DHCP** (Dynamic Host Configuration Protocol) attribue automatiquement et sans erreur l'ensemble des paramètres IP (IPv4/IPv6) aux équipements du réseau.
		
- Configuration d'une interface SVI sur Commutateur (Gestion In-Band)
	Pour administrer un switch à distance (SSH/Telnet), une adresse IP doit être appliquée sur une interface virtuelle (SVI - `vlan 1` par défaut).

##### Procédure CLI Cisco IOS :



```Cisco CLI
! Passage en mode configuration d'interface SVI
Switch(config)# interface vlan 1

! Attribution de l'adresse IP et du masque
Switch(config-if)# ip address 192.168.1.20 255.255.255.0

! Activation de l'interface SVI (Obligatoire)
Switch(config-if)# no shutdown
Switch(config-if)# exit

! Définition de la passerelle par défaut (Pour accès hors du réseau local)
Switch(config)# ip default-gateway 192.168.1.1
```