```
! ===================================================
! TEMPLATE BASE - ROUTEUR CISCO IOS
! ===================================================

enable
configure terminal

! --- 1. IDENTIFICATION ---
hostname [NOM_DU_ROUTEUR]

! --- 2. SÉCURITÉ DE L'ÉQUIPEMENT ---
! Mot de passe administrateur (Enable Secret)
enable secret [MOT_DE_PASSE_ENABLE]

! Chiffrement local des mots de passe en clair (Type 7)
service password-encryption

! Désactivation de la recherche DNS automatique
no ip domain-lookup

! --- 3. BANNIÈRE DE SÉCURITÉ (MOTD) ---
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

! --- 6. CONFIGURATION DE L'INTERFACE LAN ---
interface GigabitEthernet0/0/0
 description INTERFACE_LAN_INTERNE
 ip address [IP_INTERFACE_LAN] [MASQUE_SOUS_RESEAU]
 no shutdown
 exit

! --- 7. CONFIGURATION DE L'INTERFACE WAN ---
interface GigabitEthernet0/0/1
 description INTERFACE_WAN_VERS_ROUTER_ISP
 ip address [IP_INTERFACE_WAN] [MASQUE_SOUS_RESEAU]
 no shutdown
 exit

! --- 8. ROUTE PAR DÉFAUT (Optionnel) ---
! ip route 0.0.0.0 0.0.0.0 [IP_NEXT_HOP_ISP]

! --- 9. FINALISATION ET SAUVEGARDE ---
end
copy running-config startup-config
```