#sécurité #cybersecurité 


> [!WARNING] AVERTISSEMENT LÉGAL ET ÉTHIQUE
> Le contenu de ce cours (outils, vulnérabilités, tests d'intrusion) est présenté à des fins **strictement pédagogiques**.
> - **Laboratoires dédiés uniquement :** Toute manipulation doit être effectuée sur les environnements de TP autorisés. 
> - **Rappel de la loi :** Tester ou attaquer un système sans autorisation écrite préalable constitue un **délit pénal**.
> - L'enseignant et l'établissement déclinent toute responsabilité en cas d'usage abusif ou illégal hors du cours.


# Module 16 : Principes fondamentaux de la sécurité réseau (Cisco NetAcad  CCNA 1)

> [!abstract] **Fiche d'Identité du Cours**
> - **Module NetAcad** : Module 16 – Principes fondamentaux de la sécurité du réseau (ITN v7)
> - **Thématique de l'amorce** : La Cyberattaque des Frigos de Supermarché (Octobre 2026)
> - **Format** : Animation progressive 
> - **Durée estimée** : 45 à 60 minutes


## 📌 Phase 1 : Amorce & Étude du Cas Réel (15 min)

> [!danger] **Cas Pratique : L'Affaire des Frigos Connectés**
> Dans la nuit du 11 au 12 octobre 2026, plusieurs centaines de supermarchés en France (Intermarché, Coopérative U, E.Leclerc) ont subi une cyberattaque inédite.
> Des pirates ont pris le contrôle à distance des régulateurs frigorifiques (Danfoss AK-SM 800/800A) et ont forcé un cycle de dégivrage continu avec une consigne portée à 50 °C.
> **Conséquences** : Décongélation massive pendant la nuit, rupture de la chaîne du froid et millions d'euros de produits surgelés jetés.

### Les faits (Actualité) :

- TF1 version courte 
	 <iframe src="https://www.facebook.com/plugins/video.php?height=314&href=https%3A%2F%2Fwww.facebook.com%2Freel%2F2728027927632024%2F&show_text=false&width=560&t=0" width="560" height="314" style="border:none;overflow:hidden" scrolling="no" frameborder="0" allowfullscreen="true" allow="autoplay; clipboard-write; encrypted-media; picture-in-picture; web-share" allowFullScreen="true"></iframe>

- TF1 version longue : Comment ?
	 <iframe src="https://www.facebook.com/plugins/video.php?height=314&href=https%3A%2F%2Fwww.facebook.com%2Freel%2F1134024079150650%2F&show_text=false&width=560&t=284" width="560" height="314" style="border:none;overflow:hidden" scrolling="no" frameborder="0" allowfullscreen="true" allow="autoplay; clipboard-write; encrypted-media; picture-in-picture; web-share" allowFullScreen="true"></iframe>



- BFMTV : 
	 <iframe src="https://www.facebook.com/plugins/video.php?height=476&href=https%3A%2F%2Fwww.facebook.com%2Freel%2F1773662817010800%2F&show_text=false&width=267&t=0" width="267" height="476" style="border:none;overflow:hidden" scrolling="no" frameborder="0" allowfullscreen="true" allow="autoplay; clipboard-write; encrypted-media; picture-in-picture; web-share" allowFullScreen="true"></iframe>
	<iframe src="https://www.facebook.com/plugins/video.php?height=476&href=https%3A%2F%2Fwww.facebook.com%2Freel%2F1145023884667944%2F&show_text=false&width=267&t=0" width="267" height="476" style="border:none;overflow:hidden" scrolling="no" frameborder="0" allowfullscreen="true" allow="autoplay; clipboard-write; encrypted-media; picture-in-picture; web-share" allowFullScreen="true"></iframe>



> [!question]- 🔍 **Question **
> *« En vous appuyant sur la vidéo d'introduction , comment pouvez-vous qualifier cette attaque, quelles failles ont été exploitées et comment aurions-nous pu protéger ces équipements ? »*


## 📌 Phase 2 : Analyse Guidée – La Grille du Module 16 (20 min)

> [!info] **Matrice de correspondance : Théorie NetAcad vs. Cas Pratique**

### **Travail en équipe – « La Matrice d'Analyse d'Incident » (10 min)**

Chaque binôme reçoit une grille d'analyse à compléter en faisant le lien entre les concepts du cours  et le cas réel 


| Rubrique du Module 16                     | Concept Cisco NetAcad | Application au cas des frigos |
| ----------------------------------------- | --------------------- | ----------------------------- |
| **16.1 Types de menaces**[                |                       |                               |
| **16.1 Vulnérabilités technologiques**    |                       |                               |
| **16.1 Vulnérabilités de configuration**[ |                       |                               |
| **16.1 Vulnérabilités de politique**      |                       |                               |
| **16.2 Type d'attaque**                   |                       |                               |

### **Restitution Travail en équipe – « La Matrice d'Analyse d'Incident » (10 min)**

Chaque binôme reçoit une grille d'analyse à compléter en faisant le lien entre les concepts du cours  et le cas réel 


| Rubrique du Module 16                     | Concept Cisco NetAcad                                                                            | Application au cas des frigos                                                                                                                                           |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **16.1 Types de menaces**[                | Interruption de service (DoS) & Pertes/destruction de données et biens.                          | Déni de service physique provoquant la rupture de la chaîne du froid et la perte de denrées alimentaires.                                                               |
| **16.1 Vulnérabilités technologiques**    | Faiblesses logicielles, firmwares obsolètes ou failles d'authentification.                       | Utilisation de contrôleurs Danfoss AK-SM 800/800A tournant sur des versions de micrologiciels obsolètes contenant des failles connues (CVE-2025-41450, CVE-2025-41451). |
| **16.1 Vulnérabilités de configuration**[ | Équipements mal configurés, accès non sécurisés et exposition directe sur Internet.              | Interfaces web de supervision technique directement accessibles sur Internet sans pare-feu ni VPN[26].                                                                  |
| **16.1 Vulnérabilités de politique**      | Absence de suivi des équipements IoT/OT, manque de politiques de mise à jour.                    | Les systèmes frigorifiques étaient gérés par des prestataires externes sans audit ni suivi des correctifs par les DSI/RSSI.                                             |
| **16.2 Type d'attaque**                   | Attaque par accès (contournement d'authentification / injection de commande) et déni de service. | Exploitation d'un « code du jour » pour contourner l'authentification administrateur, suivi d'une injection de commande à distance pour forcer le dégivrage.            |


### Notions  du cours 

#### 1. Types de Menaces (Rubrique 16.1.1)
> [!note]- 👁️ **Dérouler la théorie et l'application**
> * **Théorie NetAcad** : Une intrusion peut provoquer 4 types de menaces : le vol d'informations, la perte/manipulation de données, l'usurpation d'identité et l'interruption de service (DoS).
> * **Cas réel** : L'attaque constitue un **déni de service (DoS) physique** provoqué par une **manipulation malveillante des données de consigne** de température.

#### 2. Catégories de Vulnérabilités (Rubrique 16.1.2)
> [!note]- 👁️ **Dérouler les 3 vulnérabilités NetAcad**
> Le cours Cisco identifie 3 sources de vulnérabilités :
> 1. **Technologique** : Utilisation de contrôleurs frigorifiques exécutant des versions logicielles obsolètes contenant des failles de sécurité connues (ex. CVE-2025-41450, CVE-2025-41451).
> 2. **Configuration** : Interfaces web de régulation directement exposées sur Internet sans pare-feu ni VPN.
> 3. **Politique de sécurité** : Absence d'inventaire et de gestion des correctifs sur les équipements industriels (OT/IoT), souvent gérés par des prestataires externes sans audit informatique.

[CVE-2025-41450](https://nvd.nist.gov/vuln/detail/cve-2025-41450),
[CVE-2025-41451](https://nvd.nist.gov/vuln/detail/cve-2025-41451)



> [!INFO]- **CVE**
>**Une Vulnérabilités:** C'est une **faille exploitable** qui expose le système à un risque. C'est l'équivalent d'une porte laissée entrouverte ou d'une serrure fragile qu'un tiers peut utiliser pour s'introduire.
>
> Une **CVE** (pour _Common Vulnerabilities and Exposures_, ou « Vulnérabilités et expositions communes » en français) est un **catalogue public et standardisé de failles de sécurité** informatiques. Registre mondial qui répertorie tous les bugs ou failles de sécurité découverts dans des logiciels, des systèmes d'exploitation ou des équipements matériels.
> En bref :
> - **Un identifiant standardisé** (ex. `CVE-2025-41450`) :  un **numéro d'immatriculation unique** attribué à une faille de sécurité informatique, pour désigner une vulnérabilité précise sans confusion.
> - **Un langage commun** pour que la communauté cybersécurité, les éditeurs et les entreprises parlent du même problème.
> - **Un outil de suivi** pour savoir si un logiciel contient un bug connu et s'il existe un correctif

Sites
	- **[CVE.org](https://cve.mitre.org/)** : Le site officiel et source d'origine, géré par le MITRE, qui attribue et répertorie tous les identifiants CVE.
	- **[NVD (National Vulnerability Database)](https://nvd.nist.gov/)** : La base de données du gouvernement américain (NIST), la plus utilisée pour trouver les détails techniques, les scores de gravité (CVSS) et les logiciels touchés.


> [!INFO]- **faille _zero-day_**
>**Une faille** : C'est une **erreur de conception**, de programmation ou de configuration dans un logiciel, un système ou un équipement réseau.
>
> Une **faille _zero-day_** (ou jour zéro) est une faille de sécurité informatique qui est **inconnue du créateur du logiciel** et pour laquelle il n'existe **aucun correctif**.
> En bref :
> - **Le piège :** Les pirates l'exploitent _avant_ que le fabricant ne soit au courant ou n'ait le temps de la réparer (le compteur est à « zéro jour » de correction).
> - **Le danger :** Elle est redoutable car aucun antivirus ou mise à jour ne peut en protèger au moment de l'attaque.
> - **La suite :** Dès qu'elle est découverte et rendue publique, elle reçoit généralement un numéro **CVE**, et le fabricant se précipite pour créer un correctif en urgence.
> 
> Puisqu'une faille **zero-day** est par définition inconnue et non corrigée au moment où elle est exploitée, il n'existe pas de « catalogue magique » qui l'annonce à l'avance.
> Cependant, on la découvre ou on la suit via des sources spécialisées dès qu'elle est rendue publique ou repérée en train d'être exploitée

- Sites 
	- **Le catalogue KEV de la CISA (Known Exploited Vulnerabilities) :** C'est la référence officielle (gouvernementale américaine) la plus fiable. Elle liste uniquement les failles zero-day qui ont été ou sont **activement exploitées** par des pirates dans le monde réel.
	
	- **Google Project Zero :** Une équipe de chercheurs d'élite de Google dont le travail est de traquer et de publier les détails techniques des failles zero-day les plus complexes (systèmes mobiles, navigateurs). 
	
	- **La Zero Day Initiative (ZDI) de Trend Micro :** Un organisme qui achète et suit les vulnérabilités avant leur correction officielle et analyse les bulletins de sécurité dès leur sortie.  
	
	- **Les sites d'actualité en cybersécurité :** Des médias comme _BleepingComputer_ ou _The Hacker News_ relaient en temps réel l'apparition de nouvelles failles zero-day et l'urgence de les patcher.




#### 3. Types d'Attaques Réseau (Rubrique 16.2)


> [!note]- 👁️ **Dérouler le vecteur d'attaque**
> * **Attaque de reconnaissance** : Scan d'adresses IP pour repérer les interfaces d'administration exposées.
> * **Attaque par accès** : Contournement de l'authentification ("code du jour") puis injection de commandes système.

> [!note]- **Une attaque**
> * C'est l'**action concrète** menée par un pirate (ou un outil comme Metasploit) pour exploiter cette vulnérabilité dans le but de nuire, de voler des données ou de prendre le contrôle du système.

> [!tip] 💡 En résumé 
>  * **La faille** est l'erreur initiale. 
>  * **La vulnérabilité** est la brèche qui en résulte. 
>  * **L'attaque** est l'utilisation active de cette brèche.

## 📌 Phase 3 : Plan d'Atténuation & Sécurisation (15 min)

> [!success] **Mesures de Protection (Rubriques 16.3 & 16.4)**

| Concept NetAcad | Objectif Cisco | Action concrète pour les magasins |
| :--- | :--- | :--- |
| **Approche Défense en Profondeur** | Multiplier les couches de sécurité | Isoler le réseau industriel/OT des accès Internet généraux. |
| **Pare-feu & VPN** | Contrôler le trafic et chiffrer l'accès | Interdire l'accès web direct et imposer un **tunnel VPN sécurisé** pour la télémaintenance. |
| **Mise à jour et correctifs** | Éliminer les vulnérabilités | Appliquer les correctifs logiciels fournis par le constructeur (ex. firmware R4.3.1). |
| **Durcissement des équipements** | Restreindre les services et sécuriser l'accès | Activer **SSH**, appliquer des mots de passe complexes et désactiver les services inutilement ouverts. |

## 📌 Phase 4 : Transition vers le TP Packet Tracer (5 min)

> [!tip] **Configuration IOS de base (Rubrique 16.4)**
> Projetez les commandes Cisco IOS de sécurisation étudiées dans la vidéo :
> ```cisco
> R1(config)# service password-encryption
> R1(config)# security passwords min-length 8
> R1(config)# login block-for 120 attempts 3 within 60
> R1(config)# line vty 0 4
> R1(config-line)# transport input ssh
> ```
> 👉 **Activité pratique suivante** : *Packet Tracer 16.4.6 – Configuration de mots de passe sécurisés et de SSH*.



## 📌 Pour aller plus loin : La réglementation 

> [!note] NIS2
> La **Directive (UE) 2022/2555** (connue sous le nom de **NIS2** ou **SRI 2**) est la ==réglementation européenne relative à la cybersécurité==, transposée en Belgique par la **loi du 26 avril 2024**. Elle est entrée en application le **18 octobre 2024**, remplaçant l'ancienne directive NIS1

> [!note] CCB
> Le [Centre pour la Cybersécurité Belgique](https://ccb.belgium.be/fr) est l’autorité nationale en charge de la cybersécurité en Belgique. Il supervise, coordonne et veille à la mise en œuvre de la stratégie belge en matière de cybersécurité. Grâce à un échange d’informations optimal, les entreprises, les autorités, les opérateurs de services essentiels et les citoyens peuvent compter sur une protection adéquate.

> [!note] CyFun
> Le CyberFundamentals Framework - CyFun® ==est un référentiel de cybersécurité développé par le CCB pour aider les organisations à protéger leurs données, réduire les cyberattaques et se conformer à la directive NIS2==




