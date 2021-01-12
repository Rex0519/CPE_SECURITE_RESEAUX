#CR2

base.conf
no privilage level 15

enable password

enable secret

5 Vigenère cipher
7 md5
8 sha256
9 crypted

enable algorithm-type md5 sha256 crypted


### implement username pour login mode enable
dans le conf t

'#username Fabiola algorithm-type scrypt  secret fabiola123'
 mais on arrive pas en mode login avec username, il faut l'activer dans 'line console 0', saisie la commande 'login local'

 ![show users and show line](/2_Mardi/pic/shusers_shline.png)

 'show users' 


 ### banner information
 syntax
'banner motd | exec | login'

Message of the Day (MOTD): This type of logon message has been around for a long time on Unix and mainframe systems. The idea of the message is to display a temporary notice to users, such as issues with system availability.

However, because the message displays when a user connects to the device prior to login, most network administrators are now using it to display legal notices regarding access to the switch, such as unauthorized access to this device is prohibited and violators will be prosecuted to the full extent of the law and other such cheery endearments.

Login: This banner is displayed before login to the system, but after the MOTD banner is displayed. Typically, this banner is used to display a permanent message to the users.

Exec: This banner displays after the login is complete when the connecting user enters User EXEC mode. Whereas all users who attempt to connect to the switch see the other banners, only users who successfully log on to the switch see this banner, which can be used to post reminders to your network administrators.
