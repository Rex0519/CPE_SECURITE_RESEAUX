#CR5




### ACLS AVANCÉES

#### 1. ACL DYNAMIQUE

L'ACL dynamique refuse initialement le passage du paquet de données correspondant de l'utilisateur. Lorsque l'utilisateur est authentifié avec succès, les données sont temporairement libérées, mais une fois la session terminée, l'ACL est restaurée dans sa configuration d'origine. Pour définir le moment où l'ACL dynamique est restaurée dans la configuration d'origine, nous pouvons définir le délai d'expiration de la session, on sort s'il arrive le temps d'attende sans opération, ou nous pouvons aussi définir l'heure absolue, la session doit être déconnectée après l'heure spécifiée.

'access-list 100 dynamic ccie timeout 120 permit icmp any any' Pour les données qui peuvent passer après l'authentification, telles que ICMP, le temps absolu est de 2 minutes.

!!!attention: login local pour les lines est obligatoire.

#### 2. ACL RÉFLEXIVE

Cette ACL est intéressant pour moi. ça a l'air comme un ESTABLISHED avancé.

Le principe de fonctionnement est de juger le trafic de l'extérieur, s'il s'agit du trafic de retour du réseau interne, il peut être libéré, et s'il s'agit du trafic interne généré par une source externe, il est rejeté.

ACL RÉFLEXIVE enregistrera l'état de chaque connexion, puis libérera le trafic inverse légal basé sur l'enregistrement.

Cependant, l'ACL réfléchissant a ses limites.

Il ne peut rien faire pour les services avec des trafics aller et retour différentes (tels que FTP actif, NFS, etc).

Sinon, comment vendre le pare-feu?

'Router(config-ext-nacl)#permit ip any 192.168.2.0 0.0.0.255 reflect back_to_1 timeout 30 % Autorisez 192.168.1.0/24 à accéder à 192.168.2.0/24, et lorsque l'entrée correspond au trafic, ajoutez automatiquement une entrée autorisée inversée avec un délai d'expiration de 30 s à l'ACL réfleive nommée back_to_1'

'Router(config-ext-nacl)#evaluate back_to_1  % Mappez toutes les entrées de l'ACL réflesive nommée back_to_1 à cette ACL'

#### 3. ACL BASÉ SUR TEMPS

Évidemment, l'ACL est déterminée en fonction du temps

'time-range a-name'

'periodic Monday ... h:mm to h:mm'

'access-list 101 permit udp IP MASK any dns time-range a-name'

### Pare-feu


#### Il existe plusieurs avantages a utiliser un pare-feu dans un reseau:

 * Empecher I'exposition des hotes, des ressources et des applications sensibles aux utilisateurs non approuves.

 * nspecter le flux de protocole, ce qui empeche l'exploitation des failles de protocole. Bloquer les donnees malveillantes des serveurs et des clients.

 * Reduire la complexite de la gestion de la securite en dechargeant la majeure partie du controle d'acces au reseau sur quelques pare-feu du reseau.



#### Attention Les pare-feu presentent egalement certaines limitations:

 * Un pare-feu mal configure peut avoir de graves consequences pour le reseau, comme devenir un point de defaillance unique.

 * Les donnees de nombreuses applications ne peuvent pas etre transmises via les pare-feu en toute securite.

 * Les utilisateurs peuvent rechercher de maniere proactive des moyens de contourner le pare-feu pour recevoir du contenu bloque, ce qui expose le reseau a une attaque potentielle.

 * Les performances du reseau peuvent ralentir.

 * Le trafic non autorise peut etre tunnelise ou masque en tant que trafic legitime a travers le pare-feu.


#### PARE-FEU DE NOUVELLE GENERATION

 Un pare-feu de nouvelle generation va au-dela du pare-feu avec etat de plusieurs manieres importantes:

 * Identification, visibilite et controle granulaires des comportements dans les applications

 * Restreindre I'utilisation du Web et des applications Web en fonction de la reputation du site

 * Protection proactive contre les menaces Internet

 * Application des politiques en fonction de l'utilisateur, de lappareil, du role, du type d'application et du profil de menace

 * Performance de NAT, VPN et inspection de protocole avec etat (SPI) Utilisation d'un systeme integre de prevention des intrusions (IPS)

![newparfeu](/5_Vendredi/pic/newparfeu.png)

#### ZPF 

a la futur


#### PFSENSE

 LE FIREWALL PFSENSE

 Le projet pfSense, est base sur un fork de mOnOwall realise en 2004 par Chris Buechler et Scott Ullrich11.

 pfSense est un routeur/ pare-feu open source base sur le systeme d'exploitation FreeBSD.

 A I'origine un fork de mOnOwall, il utilise le pare-feu etats Packet Filter,des fonctions de routage et de NAT lui permettant de connecter plusieurs reseaux informatiques.

 Il comporte l'equivalent libre des outils et services utilises habituellement sur des routeurs professionnels proprietaires.

 pfSense convient pour la securisation d'un reseau domestique ou d'entreprise.

##### L'installation 

0. LAN segment dans conf de VMWARE Workstation

1. configuration des address IP pour WAN et LAN sur 1 et 2

2. configuration l'accès WAN

' pfSsh. php playback enableallowallwan'

##### La configuration de chaque service

1. SSH
2. NAT
3. ALLIASES
4. VIRTUAL IPs
5. TRAFFIC SHAPER(limite débit)

##### Les outils (PLUGIN EXTRA)

* NTOPNG (outil d'analyse)

* PFBLOCKERNG  protection de la vie privee et de tracage avec filtrage Web permettant d'ameliorer la confidentialite, de controler les acces, de supprimer les publicites et de bloquer l'acces aux sites publicitaires.

##### MULTI-WAN pour plusieur LSPs ou MSOs

1. la balance de la charge

2. l'Agrégation de liens

##### Redondance

 * High Availabillty Sync
 	PFSYNC

 * Common Address Redundancy Protocol (CARP)
 open redundancy solution for sharing IP addresses among a group of network devices. Similar solutions already existed, primarily the IETF standard for Virtual Router Redundancy Protocol (VRRP). However Cisco claims VRRP is covered by its patent on their Hot Standby Router Protocol (HSRP), and told the OpenBSD developers that it would enforce its patent. 

###	IDS ET IPS

A VENIR

### Some tricks

simulation un VPC par un routeur:

no ip route

ip default-gateway a.b.c.d


rewrewr 