# Module 13 : Messages ICMP

## 1 Messages ICMPv4 et ICMPv6

### 1.1. Vue d'ensemble
* **Rôle :** 
	* Fournir des informations en retour (feedback) sur le traitement des paquets IP en cas d'erreur ou d'information opérationnelle.
* **Sécurité :** 
	* Souvent filtrés ou bloqués aux frontières du réseau pour des raisons de sécurité.
* **Types de messages communs à ICMPv4 et ICMPv6 :**
	  1. Accessibilité de l'hôte (*Host Reachability* - Ping)
	  2. Destination ou Service Inaccessible (*Destination/Service Unreachable*)
	  3. Délai dépassé (*Time Exceeded*)


### 1.2. Messages d'erreur et de contrôle courants

#### A. Accessibilité de l'hôte
* **Demande d'écho (Echo Request) & Réponse d'écho (Echo Reply) :** Permettent de tester la connectivité de couche 3 entre deux hôtes (base de la commande `ping`).

#### B. Destination ou Service inaccessible
Envoyé par un hôte ou un routeur lorsqu'un paquet ne peut pas être acheminé. Contient un code d'erreur explicatif :

| Code ICMPv4 | Signification | Code ICMPv6 | Signification |
| :---: | :--- | :---: | :--- |
| **0** | Réseau inaccessible | **0** | Pas de route vers la destination |
| **1** | Hôte inaccessible | **1** | Interdit administrativement (ex: Pare-feu) |
| **2** | Protocole inaccessible | **2** | Au-delà de la portée de l'adresse source |
| **3** | Port inaccessible | **3** | Adresse inaccessible |
| **-** | - | **4** | Port inaccessible |

#### C. Délai dépassé / Time Exceeded
* **Mécanisme :** Envoyé par un routeur lorsque la durée de vie d'un paquet expire :
  * **IPv4 :** Le champ **TTL** (*Time To Live*) atteint `0`.
  * **IPv6 :** Le champ **Hop Limit** (limite de sauts) atteint `0`.
* **Application :** Utilisé par l'outil `traceroute` / `tracert` pour identifier chaque saut du chemin réseau.


### 1.3. Protocoles et Messages spécifiques à ICMPv6 (NDP)

ICMPv6 intègre le protocole **NDP (*Neighbor Discovery Protocol*)** qui gère la communication entre équipements IPv6 sur le même lien :

> [!ABSTRACT] Les 4 Messages Cés de NDP (ICMPv6)

#### 1. RA (Router Advertisement)
* Envoyé par les routeurs (périodiquement toutes les 200s ou sur demande RS).
* Fournit le préfixe réseau, la longueur de préfixe (`/64`), la passerelle par défaut (adresse LLA du routeur) et les instructions d'adressage (SLAAC / DHCPv6).

#### 2. RS (Router Solicitation)
* Envoyé par un hôte au démarrage (à l'adresse multicast `ff02::2` - Tous les routeurs) pour demander immédiatement un message RA.

#### 3. NS (Neighbor Solicitation)
* **Résolution d'adresse :** Demande l'adresse MAC d'un voisin dont l'IP est connue (remplace la requête ARP d'IPv4). Envoyé à l'adresse multicast de nœud sollicité.
* **DAD (Duplicate Address Detection) :** Vérifie l'unicité d'une adresse IPv6 avant attribution en envoyant un NS ciblant sa propre IP prospective.

#### 4. NA (Neighbor Advertisement)
* Réponse au message NS. Contient l'adresse MAC de l'équipement émetteur pour finaliser la résolution d'adresse ou signaler une IP déjà occupée lors du DAD.

## 2 Tests à l'aide des commandes ping et traceroute

### 2.1. La commande Ping - Diagnostics de connectivité 

`ping` utilise des messages **ICMP Echo Request** et **Echo Reply** pour tester l'accessibilité d'un hôte et mesurer le temps d'aller-retour.

> [!TIP] Démarche méthodique de dépannage avec `ping`

#### Étape 1 : Test du bouclage local (Loopback)
* **Commandes :** 
	* `ping 127.0.0.1` (IPv4) ou `ping ::1` (IPv6)
* **Objectif :** 
	* Vérifie que la suite de protocoles **TCP/IP est correctement installée** et fonctionnelle sur l'hôte local.
* **Limite :** 
	* N'indique pas si la carte réseau physique ou la configuration IP (masque/passerelle) est correcte.

#### Étape 2 : Test de la passerelle par défaut
* **Commande :** 
	* `ping <IP_Passerelle>` (adresse IP/LLA de l'interface du routeur local)
* **Objectif :** 
	* Confirme que l'hôte peut communiquer sur son **réseau local (LAN)** et que l'interface du routeur local fonctionne.

#### Étape 3 : Test d'un hôte distant
* **Commande :** 
	* `ping <IP_Hôte_Distant>`
* **Objectif :** 
	* Valide la connectivité globale à travers l'interréseau (routeurs intermédiaires opérationnels).
* **Remarque :** 
	* Si le ping échoue, cela peut être dû à un problème de routage ou à des règles de sécurité (pare-feu bloquant ICMP).


### 2. La commande Traceroute / Tracert (13.2.5)
[[Module 08 Couche reseau#5. Paquet IPv4]]
* **Rôle :** 
	* Génère la liste de tous les routeurs (sauts) traversés par un paquet pour atteindre une destination.
* **Mécanisme (TTL / Hop Limit) :**
  1. Envoie une première séquence de paquets avec **TTL = 1** (IPv4) ou **Hop Limit = 1** (IPv6). Le premier routeur décrémente la valeur à `0`, abandonne le paquet et renvoie un message **ICMP Time Exceeded**.
  2. Incrémente progressivement le TTL (2, 3, 4...) pour identifier chaque saut successif.
  3. Lorsque la destination finale est atteinte, elle répond par un message **ICMP Port Unreachable** ou **Echo Reply**.
* **Affichage des temps (RTT) :** Affiche le temps d'aller-retour pour chaque saut. Un astérisque (`*`) indique un délai dépassé ou un paquet bloqué.