```
! ===================================================
! TEMPLATE BASE - SWITCH CISCO IOS
! ===================================================

enable
configure terminal

! --- 1. IDENTIFICATION ---
hostname [NOM_DU_SWITCH]

! --- 2. SÉCURITÉ DE L'ÉQUIPEMENT ---
! Mot de passe administrateur (Enable Secret)
enable secret [MOT_DE_PASSE_ENABLE]

! Chiffrement local des mots de passe en clair (Type 7)
service password-encryption

! Désactivation de la recherche DNS automatique (évite les blocages sur erreur de frappe)
no ip domain-lookup

! --- 3. BNIÈRE DE SÉCURITÉ (MOTD) ---
banner motd #
===================================================
  ATTENTION: Accès réservé au personnel autorisé!
  Toute tentative d'accès non autorisé sera poursuivie.
===================================================
#

! --- 4. ACCÈS CONSOLE (Out-of-Band) ---
line console 0
 password [MOT_DE_PASSE_CONSOLE]
 login
 logging synchronous
 exit

! --- 5. ACCÈS DISTANT VTY (In-Band) ---
line vty 0 15
 password [MOT_DE_PASSE_VTY]
 login
 logging synchronous
 exit

! --- 6. GESTION IN-BAND (SVI VLAN 1) ---
interface vlan 1
 description INTERFACE_MANAGEMENT
 ip address [ADRESSE_IP_SWITCH] [MASQUE_SOUS_RESEAU]
 no shutdown
 exit

! Passerelle par défaut pour la gestion à distance
ip default-gateway [IP_PASSERELLE_DEFAUT]

! --- 7. FINALISATION ET SAUVEGARDE ---
end
copy running-config startup-config
```