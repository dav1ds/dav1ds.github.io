# SELinux


sestatus pour conaitre les 3 modes  d'activation de selinux 

Enforcing : actif
Permissive: non actif mais log les action ~ mode debug  
Disabled : desactivé . L'acces n'est pas restreint

    getenforce : nous informe du mode de selinux

    setenforce 0 : bascule temporairement jusqu'au prochain redemarrage en Permissive ( si mode Enforcing actif)

Mode par defaut defini dans /etc/selinux/config

```
# /etc/selinux/config
SELINUX=enforcing
SELINUXTYPE=targeted
```

On garde targeted, qui garantit la surveillance des principaux services réseau.


Lorsqu'on passe du mode Disabled à Enforcing , il faut reetiquetter l'ensemble des fichiers du systeme 
Il faut creer un fichier vide à la racine du systeme avant de redemarrer
# touch /.autorelabel
# reboot

( peu prendre un certain temps)

# Principe: 

Basé sur le principe de label 

user:role:type:context


- les fichiers et les processus sont etiquetés avec un contexte de sécurité:

- Les processus surveill&s pae selinux doivent respecter un certain nombre de regles 

L'option -Z permet d'afficher  le contexte de securité 

```
cd /var/www/html/
# ls -Z
unconfined_u:object_r:httpd_sys_content_t:s0 index.html
```

Idem avec ps:

```
# ps -axZ | grep httpd
system_u:system_r:httpd_t:s0  3948 ?  Ss  0:00 /usr/sbin/httpd -DFOREGROUND
system_u:system_r:httpd_t:s0  3949 ?  S   0:00 /usr/sbin/httpd -DFOREGROUND
```

Contexte de securité SElinux :
user:role:type:context


La commande restorecon ( restore context) restaure le contexte de sécurité /reetiquette un fichier 
 
restorecon -Rv /var/www/html/

Restauration du contexte SElinux pour reetiqueter correctement tout le contenu du répertoire /var/www/html.


En mode Strict comme en mode permissif , chaque fichier nouvellement créer à un certian endroit du systeme de fichier sera correctement labelisé 


matchpathcon permet de nous renseigner sur l'etiquette utilisé en fonction du repertoire 

```
$ matchpathcon /var/www/html/
/var/www/html   system_u:object_r:httpd_sys_content_t:s0
$ matchpathcon /var/log/httpd/
/var/log/httpd  system_u:object_r:httpd_log_t:s0
$ matchpathcon /etc/httpd/conf/
/etc/httpd/conf system_u:object_r:httpd_config_t:s0

```

Changement du contexte pour un fichier 
```
# semanage fcontext -a -t httpd_sys_content_t '/srv/web(/.*)?'
# restorecon -Rv /srv/web/
```

- (-a) : ajout d'une entré 
- (-t) d'un type http_sys_content_t
- L'expression reguliere  applique la directive recursive  '/srv/web(/.*)?'

 restorecon applique le context recursivement sur le repertoire

La documentation se SElinux cite chcon pour modifier le contexte d'un  fichier ou repertoire 
Le problème avec chcon, c’est qu’à la prochaine utilisation de restorecon sur le fichier ou ses parents, le fichier sera réétiqueté avec le contexte de son plus proche parent pour lequel un contexte spécifique est défini.

IL vaut mieux utiliser semanage fcontext et restorecon

## Booleen 
boolean  : ON/OFF switch
Un boolean est une action que peut faire un service sur un autre 
IL y en a plus de 300 

getsetbool -a  ( pour  avoir la liste de boleens)
ou semanage boolean -l


Pour activer /desactiver des booleens !
setsebool -P **boolean** on/off

Pour afficher les booleens personnalisés

# semanage boolean -lC 
SELinux boolean                State  Default Description
httpd_enable_homedirs          (on   ,   on)  Allow httpd to enable homedirs

Pour changer un label: 
    chcon -t httpd_sys_content_t filemanm
    semanage -t httpd_sys_content_t filemanm

# Bonus 

Installer le paquet setroubleshoot-server pour ameliorer la lisibilité des logs liés à SElinux 
```
sealert -a /var/log/audit/audit.log | less

100% done
found 1 alerts in /var/log/audit/audit.log
--------------------------------------------------------
SELinux is preventing /usr/sbin/httpd from getattr access 
on the file /var/www/html/index.html.
***** Plugin restorecon (94.8 confidence) suggests *****
If you want to fix the label. 
/var/www/html/index.html default label should be httpd_sys_content_t.
Then you can run restorecon. The access attempt may have been stopped 
due to insufficient permissions to access a parent directory in which 
case try to change the following command accordingly. Do
# /sbin/restorecon -v /var/www/html/index.html
...
```
Traduction : 
SELinux empêche Apache d’accéder au fichier index.html.
L’étiquette par défaut devrait être httpd_sys_content_t.
La commande restorecon permet de rétablir l’étiquette correcte.
