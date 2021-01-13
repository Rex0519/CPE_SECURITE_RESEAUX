#CR3


### Limitation de la disponibilité des commandes

#### configuration et attibution de niveaux de privilège

on peut le réaliser par 3 étapes

1. modifier level privilège de la commande qu'on veut.
'privilege exrc level 5 ping'

2. créer un MDP pour change l'exec level pour l'user temporellement.
'enable algorithm_type scrypt secret level 5 cisco5'
pour accéder au level on veut: 'enable 15' et saisir le MDP pour avoir le level on veut.

3. créer un compte ayant un level privilege on le donne.
'username support privilege 5 algorithm_type scrypt secret cisco5'

#### configuration de view et superview

mode view est un autre façon pour distribuer le droits aux users.
la différence est:le mode view distribue les droits de commande à un ou plusieur groupe.
Si une commande n'a pas configuré dans un view, elle ne peut pas être exécuté.



### système log de routeur

une autre VM est nécessaire pour stocker le syslog. dans ce cas-ci, on installe un système linux 'net-toolbox'

sur routeur:
'logging host *IP_Address*'

'logging trap *level*' usuellement 7- debugging

'logging on' activer logging



#### service NTP POUR SYNCRONISER LE LOG
un horloge synchronisé est demandé pour le système log.

Les secondes comptent lorsqu'ils'agit d'une attaque, car il est important d'identifier l'ordre dans lequel une attaque spécifiée s'est produite.

![topologie_ntp](/3_Mercredi/pic/topologientp.PNG)


* syntax

1. 'ntp master *stratum*'
*stratum* est l'hop de routage.

2. 'ntp server ip' désigner le serveur ntp par l'adress IP.

3. 'ntp broadcast client' configuré sur des interfaces routeur.

apres la configuration, on peut voir le resultat par 'show ntp status'

![ntpr2](/3_Mercredi/pic/ntpr2.PNG)

### serveur tacacs/radius

#### preparation

installation des machines TACACS, WEBTERM, deux ROUTEUR(user et ROUTER).

![topologie_serveur_tacacs](/2_Mercredi/pic/topologietacacs.PNG)

#### Configurations:

```
(sur le routeur)
username wuqi password cisco5
aaa new-model
!
aaa groupe server tacacs+ gns3group
  server name tacacsgui
!
aaa authentication login default group gns3group local

!
tacacs server tacacsgui
  address ipv4 10.0.0.10
  key cisco

!
enable password cisco ###pour  activer mode enable pour la connexion AAA
```


#### DANS gui tacacs(sur webterm)


* Ajoute un group devices

* Ajoute device ROUTER

* Ajoute un user avec un banner gentil

* atttention: tous les MDPs sont clair-text

* Après chaque modification, il faut bien 'apply changes' déchochant 'make backup after applying'. En ravanche, il ne marche pas et faut tout refaire.


![nobackup](/3_Mercredi/pic/nobackuptacacs.PNG)

#### Aller sur ROUTER
```
line vty 0 4
  tranport input telnet
!
'#'debug tacacs
```
#### aller sur Router user
```
telnet 10.0.0.4 (utiliser ID et MDP enregistrés sur TACACS GUI)
```
aller sur ROUTER puis d'analyser les logs

#### blocage de la connection serveur tacacs

si l'on desconnecte le serveur tacacs, USER peut encore acceder au serveur par vty.
c'est a dire que l'authetication ne marche que le premiere fois.
