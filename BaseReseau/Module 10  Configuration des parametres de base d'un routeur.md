# Module 10 : Configuration des paramètres de base d'un routeur


## 1. Paramètres initiaux du routeur

- **Nom du périphérique :** 
	- Attribuer un nom d'hôte unique.
    
- **Accès au mode EXEC privilégié :** 
	- Sécuriser l'accès avec un mot de passe chiffré (`enable secret`).
    
- **Accès au mode EXEC utilisateur (Console) :** 
	- Sécuriser la ligne de console physique.
    
- **Accès à distance (Lignes VTY) :** 
	- Sécuriser les accès SSH / Telnet.
    
- **Chiffrement des mots de passe :** 
	- Chiffrer tous les mots de passe en texte clair dans le fichier de configuration.
    
- **Avertissement légal :** 
	- Configurer une bannière MOTD (_Message of the Day_).
    
- **Sauvegarde :** 
	- Enregistrer la configuration active de la RAM vers la NVRAM.
- Voir : 
	- [[Module 2 Config de base Switch]]

## 2. Exemple complet de configuration (Routeur R1)



``` IOS
! 1. Passage en mode configuration globale et nommage
Router> enable
Router# configure terminal
Router(config)# hostname R1

! 2. Sécurisation de l'accès EXEC privilégié
R1(config)# enable secret class

! 3. Sécurisation de la ligne console
R1(config)# line console 0
R1(config-line)# password cisco
R1(config-line)# login
R1(config-line)# exit

! 4. Sécurisation des lignes VTY (accès à distance)
R1(config)# line vty 0 4
R1(config-line)# password cisco
R1(config-line)# login

! Nouveau
R1(config-line)# transport input ssh telnet
R1(config-line)# exit
! Pourquoi le exit Ici ?

! 5. Chiffrement de tous les mots de passe en texte clair
R1(config)# service password-encryption

! 6. Configuration de la bannière légale (MOTD)
R1(config)# banner motd #
Enter TEXT message. End with a new line and the #
*************************************************
WARNING: Unauthorized access is prohibited!
*************************************************
#

! 7. Enregistrement de la configuration dans la NVRAM
R1(config)# end
R1# copy running-config startup-config
```

## 3. Configuration des interfaces

#### 3.1 Étapes de configuration d'une interface de routeur

- **Rôle :** Activer et adresser les interfaces physiques (ex: `GigabitEthernet 0/0/0`, `GigabitEthernet 0/0/1`) pour permettre le routage des paquets.
        
- **Paramètres essentiels :**   
      
    - **Description :** Optionnelle mais recommandée (limite à 240 caractères) pour documenter le rôle ou le raccordement de l'interface.       
          
    - **Adresse IPv4 / IPv6 :** Configuration des adresses de couche 3.
          
        
    - **Activation (`no shutdown`) :** Par défaut, les interfaces d'un routeur sont désactivées (_administratively down_). La commande `no shutdown` équivaut à la mise sous tension de l'interface.
        



```IOS
Router(config)# interface <type-et-numéro>
Router(config-if)# description <texte-descriptif>
Router(config-if)# ip address <adresse-ipv4> <masque-sous-réseau>
Router(config-if)# ipv6 address <adresse-ipv6/longueur-préfixe>
Router(config-if)# no shutdown
```

#### 3.2 Exemple de configuration des interfaces (Routeur R1)

Plaintext

```
! Configuration de l'interface Gig0/0/0 (LAN local)
R1> enable
R1# configure terminal

R1(config)# interface gigabitEthernet 0/0/0
R1(config-if)# description Link to LAN
R1(config-if)# ip address 192.168.10.1 255.255.255.0
R1(config-if)# ipv6 address 2001:db8:acad:10::1/64
R1(config-if)# no shutdown
R1(config-if)# exit

! Messages du système confirmant le changement d'état :
! %LINK-3-UPDOWN: Interface GigabitEthernet0/0/0, changed state to up
! %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/0, changed state to up

! Configuration de l'interface Gig0/0/1 (Lien vers R2)
R1(config)# interface gigabitEthernet 0/0/1
R1(config-if)# description Link to R2
R1(config-if)# ip address 209.165.200.225 255.255.255.252
R1(config-if)# ipv6 address 2001:db8:feed:224::1/64
R1(config-if)# no shutdown
R1(config-if)# exit
```

[[Router_Template]]
#### 3.3 Commandes de vérification de la configuration

|**Commande**|**Description**|
|---|---|
|**`show ip interface brief`**|Affiche l'état synthétique IPv4 (Adresse IP, Statut, Protocole) de toutes les interfaces.|
|**`show ipv6 interface brief`**|Affiche l'état synthétique IPv6 (Adresses Link-Local et GUA, Statut).|
|**`show ip route`** / **`show ipv6 route`**|Affiche le contenu de la table de routage stockée en RAM.|
|**`show interfaces`**|Affiche les statistiques détaillées et l'adressage IPv4/MAC de toutes les interfaces.|
|**`show ip interface`** / **`show ipv6 interface`**|Affiche les paramètres et statistiques spécifiques IPv4 ou IPv6 des interfaces.|


```IOS
R1# show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0/0   192.168.10.1    YES manual up                    up      
GigabitEthernet0/0/1   209.165.200.225 YES manual up                    up      
Vlan1                  unassigned      YES unset  administratively down down    

R1# show ipv6 interface brief
GigabitEthernet0/0/0   [up/up]
    FE80::201:C9FF:FEB9:4501
    2001:DB8:ACAD:10::1
GigabitEthernet0/0/1   [up/up]
    FE80::201:C9FF:FEB9:4502
    2001:DB8:FEED:224::1
```

### 4 Passerelle par défaut

#### 4.1 Passerelle par défaut pour un hôte

- **Définition :** 
	- L'adresse IP de l'interface du routeur local connectée au même segment LAN que l'hôte.   
    
- **Rôle :** 
	- Utilisée uniquement lorsqu'un hôte doit transmettre des paquets vers un réseau distant. Pour les communications sur le même réseau local, les hôtes échangent directement via le Switch sans solliciter la passerelle.
    

#### 4.1 Passerelle par défaut pour un switch (Layer 2)

- **Pourquoi la configurer ?** 
	- Un switch de couche 2 n'a pas besoin de passerelle pour acheminer le trafic des clients. Toutefois, une passerelle par défaut est **nécessaire pour la gestion à distance** (SSH/Telnet) du switch depuis un autre réseau.
    
- **Configuration IPv4 :** 
	- Commande globale `ip default-gateway <ip-passerelle>` associant l'IP du routeur connecté.
    
- **Configuration IPv6 :** 
	- Le switch apprend automatiquement sa passerelle IPv6 par défaut via les messages ICMPv6 **Router Advertisement (RA)** reçus du routeur.


```IOS
! Configuration de l'accès de gestion et de la passerelle sur Switch S1
S1# configure terminal
S1(config)# interface vlan 1
S1(config-if)# ip address 192.168.10.50 255.255.255.0
S1(config-if)# no shutdown
S1(config-if)# exit
S1(config)# ip default-gateway 192.168.10.1
```