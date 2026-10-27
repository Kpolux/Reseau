WIFI Eleve MP: itlgwifi2018
# Connection 

MP : PgIt1013 / PgIt_1013 / ItLg1013 / ItLg_1013

Proxmox 
carte de réseau pour les VM créée 
PC sur vlan chaq Pc 6 vlans 

VMBR0 => aparait dans le réseau de classe 

Netacad 
My Organization 
- haute école de la ville de liege
créé cours et mettre sur invitation 

mettre date de debut et fin => 1 an
inviter etudiant email prive par 5 étudiants
autre technique fichier csv
alumni => chercher les anciens cours 
en tant que prof voir ce que les étudiants voi
login : mail Pol  / MP: underscore
meme pourcentage des examen : manae grade book 
il faut aussi activer les  examen pour qu'il soit disponible 


Nouvelle utilisateur admin.info pour creation des comptes étudiants MFA pour admin 
aller faire corriger le mail au sécrétariat / regarder mail envoyer 
vm serveur windows 

client totp pour 2FA



```Bash
ssh utilisateur@adresse_ip
# MDP : class
```


## POD G (192.168.2.110)
### Equipements Réseau
- **S1 :** `ssh podgs1@192.168.2.110`
    
- **S2 :** `ssh podgs2@192.168.2.110`
    
- **S3 :** `ssh podgs3@192.168.2.110`
    
- **R1 :** `ssh podgr1@192.168.2.110`
    
- **R2 :** `ssh podgr2@192.168.2.110`
    
- **R3 :** `ssh podgr3@192.168.2.110`
### End Point (PC) en web
**PC podg-1 à 3:** `https://192.168.8.250:8006`

## POD D (192.168.2.120)
- **S1 :** `ssh podds1@192.168.2.120`
    
- **S2 :** `ssh podds2@192.168.2.120`
    
- **S3 :** `ssh podds3@192.168.2.120`
    
- **R1 :** `ssh poddr1@192.168.2.120`
    
- **R2 :** `ssh poddr2@192.168.2.120`
    
- **R3 :** `ssh poddr3@192.168.2.120`

## POD I (192.168.2.130)

- **S1 :** `ssh podis1@192.168.2.130`
    
- **S2 :** `ssh podis2@192.168.2.130`
    
- **S3 :** `ssh podis3@192.168.2.130`
    
- **R1 :** `ssh podir1@192.168.2.130`
    
- **R2 :** `ssh podir2@192.168.2.130`
    
- **R3 :** `ssh podir3@192.168.2.130`
    

## POD A (192.168.2.140)

- **S1 :** `ssh podas1@192.168.2.140`
    
- **S2 :** `ssh podas2@192.168.2.140`
    
- **S3 :** `ssh podas3@192.168.2.140`
    
- **R1 :** `ssh podar1@192.168.2.140`
    
- **R2 :** `ssh podar2@192.168.2.140`
    
- **R3 :** `ssh podar3@192.168.2.140`
    

## POD T (192.168.2.150)

- **S1 :** `ssh podts1@192.168.2.150`
    
- **S2 :** `ssh podts2@192.168.2.150`
    
- **S3 :** `ssh podts3@192.168.2.150`
    
- **R1 :** `ssh podtr1@192.168.2.150`
    
- **R2 :** `ssh podtr2@192.168.2.150`
    
- **R3 :** `ssh podtr3@192.168.2.150`