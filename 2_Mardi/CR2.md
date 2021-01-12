#CR2

### AUGMENTER LA SÉCURITÉ D'ACCÈS

#### sécurité de config par défaute

 ![config_default](/2_Mardi/pic/chemin_configdefault.PNG)
'no privilage level 15'
On ajoute 'no' avant privilage level 15 pour annuler le privilage trop haute pour le vty et aux.

Après bien configuré user, il faut ajouter le permit pour vty.
par example 'transport input telnet'

'enable password' est pour créer un MDP clair. C'est à dire qu'il peut être vu directement dans le config.txt.

'enable secret' crée un MDP caché par Vigenère cipher qui est bien obscuré.

niv| nom
x<5 | Vigenère cipher
5 | md5
8 |sha256
9 |crypted

'enable algorithm-type md5 sha256 crypted' est généger un MDP le plus fort dans le système.


### implement username pour login mode enable

Si nous voulons un username et MDP pour agumenter la sécurité, on bascule à la commande suivant:

'#username Fabiola algorithm-type scrypt secret fabiola123'
 mais on arrive pas en mode login avec username, il faut l'activer dans 'line console 0', saisie la commande 'login local'

 ![show users and show line](/2_Mardi/pic/shusers_shline.PNG)

 'show users' 'show line' montre des infos des sessions on a créé.


### banner information
 syntax
'banner motd | exec | login'

dans le mode config, on saisie banner motd avec un '#' et ensuite le message qui est terminé par un autre '#'.

 ![banner_ggez](/2_Mardi/pic/ggez.PNG)

 <details>
 <summary>plus d'infos en anglais</summary>
 <pre><code>
Message of the Day (MOTD): This type of logon message has been around for a long time on Unix and mainframe systems. The idea of the message is to display a temporary notice to users, such as issues with system availability.

However, because the message displays when a user connects to the device prior to login, most network administrators are now using it to display legal notices regarding access to the switch, such as unauthorized access to this device is prohibited and violators will be prosecuted to the full extent of the law and other such cheery endearments.

Login: This banner is displayed before login to the system, but after the MOTD banner is displayed. Typically, this banner is used to display a permanent message to the users.

Exec: This banner displays after the login is complete when the connecting user enters User EXEC mode. Whereas all users who attempt to connect to the switch see the other banners, only users who successfully log on to the switch see this banner, which can be used to post reminders to your network administrators.

</code>
</pre>
</details>

#### Explication syntax
1. 'login block-for *seconds1* attempts *tries* within *seconds2*'
S'il y *tries* fois de login pendant *seconds2* secondes, bloquer le dans *seconds1* secondes

2. 'login quiet-mode access-class *acl*'
si l'adress IP de routeur est dans ce list, ignorer ce block.
syntax 'ip access-list standard *nom de ACL*', 'permit *adress-IP*'.

 ![quietmode](/2_Mardi/pic/quietmode.PNG)

3. 'login delay *seconds*' delais après chaque saisi

4. 'login on-success log' et 'login on-failure log' affiche un msg dans CLI du LHOST.

* si l'on est dans 'sh login', on peut obvserver le mode actuel: Normal-mode ou silence mode. ça depand à l'état block-for.

### sécurité SSH

le packet telnet est envoyé en clear-text. Et c'est tres facile de capter par analyseur packet

![wireshark](/2_Mardi/pic/wireshark.PNG).

On utilise ainsi SSH(secu SH).
'''
R1(config)#ip domain-name crackcpe.com

R1(config)#crypto key generate rsa general-keys modulus 2048

R1(config)#ip ssh version 2

R1(config)#username chiant algorithm-type scrypt secret chiant12345

R1(config)#line vty 0 4

R1(config-line)#login local

R1(config-line)#transport input ssh

R1(config-line)#exit
'''
![wireshark](/2_Mardi/pic/wiressh.PNG).
