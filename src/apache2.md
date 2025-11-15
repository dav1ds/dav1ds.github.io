# Apache2 

-  Le serveur HTTP Apache a été créé par Robert McCool en 1995 et développé sous la direction de Apache Software Foundation depuis 1999.

-  Serveur le plus populaire car Apache bénéficie d’une excellente documentation et d’un support intégré provenant d’autres projets.

## 

### Avantages
  -  Un serveur web Apache peut être un excellent choix pour exécuter un site web sur une plateforme stable et polyvalente.
  -  Open-source 
  -  Mise à jour régulière, correctifs de sécurité réguliers.
  -  Flexible grâce à sa structure basée sur des modules.
  -  Multi-Plateforme (Unix/Linux et Windows).
  -  Bonne documentation et large communauté d'utilisateurs


### Inconvénients :
   - Problèmes de performances sur les sites web avec beaucoup de trafic.
   
# Methode de traitement des requêtes:

3 principaux modules multiprocessus (MPM):  

Modules utilisés par le serveur web Apache HTTP Server qui définissent la manière d'accepter les requêtes des clients et  la façon de les répartir entre les différents processus.   

1. Process model: 
    - Méthode "pre-fork" initiale; 
    - Consomme beaucoup de RAM et pourrait même refuser des connexions à des charges élevées. 
    Les sites plus petits ne le remarqueront pas, mais des sites plus importants le feront probablement.
    
        
##
2. Worker model: 
    - Ce modèle crée un processus de contrôle unique qui est responsable du lancement des processus enfants.  
    Chaque processus enfant crée alors un nombre fixe de threads, ainsi qu'un thread d'écoute. 
    Le thread de l'auditeur écoute les connexions et les transmet à un thread pour le traitement à leur arrivée.   
    - Modèle plus performant (moins gourmand en ressource), il peut encore se heurter à des problèmes de mise à l'échelle pour des sites ayant un trafic très élevé, en raison du goulot d'étranglement du processus de contrôle unique.

## MPM Worker 

![](apache2_mpm_worker.png)


## 
3. Event model: 
    - Similaire au worker model, mais il crée un thread d'écoute qui écoute les connexions et les transmet à un thread de travail pour le traitement.  
    Ce MPM gère les connexions longues beaucoup plus efficacement sur un seul thread (gestion KeepAlive).  
    - Paramètre par défaut depuis Apache 2.4 


# Configuration
     
##  Arborescence Apache  

```shell
/etc/apache2/
├── apache2.conf
├── conf-available
├── conf-enabled
├── envvars
├── magic
├── mods-available
├── mods-enabled
├── ports.conf
├── sites-available
└── sites-enabled
```


## Simple !
- /etc/apache2
	- un seul fichier apache2.conf 
- Directives de configuration 
	- une par ligne
	- commentaire introduit par #
- Explicite 
    - DocumentRoot  /var/www/html

## structurée
- bloc de syntaxe à la XML        <Directory> ... </Directory>
- limite les directives contenues à un sous-ensemble des fichiers servis
    - \<Directory\>,\<Location\>, etc.
- définisse un fonctionnement
	- \<VirtualHost\>
    
## Mécanisme de répertoire de préparation 
- Available vs Enabled
- Activation d’un fragment de configuration par des liens symboliques
- Commandes:
     - a2en*=activation
     - a2dis*=désactivation
     
## Exemple de configuration 

```conf
# Apache doit écouter sur le port 80
Listen 80

# Toutes les adresses IP doivent répondre aux requêtes sur les 
# serveurs virtuels
NameVirtualHost *:80

<VirtualHost *:80>
DocumentRoot /www/example.com
ServerName www.example1.com

# Autres directives ici
</VirtualHost>

<VirtualHost *:80>
DocumentRoot /www/example.org
ServerName www.example2.org

# Autres directives ici
</VirtualHost>
```

## Conclusion
Apache est l’un des serveurs web les plus populaires qui vous permet de gérer un site web sécurisé sans trop de problèmes.    
Choix le plus fréquent des independants et des petites entreprises qui veulent une présence sur le web.  

Apache fonctionne parfaitement avec de nombreux autres systèmes de gestion de contenu CMS  
– SGC (Joomla, Drupal, etc.), les Framework web (Django, Laravel, etc.) et les langages de programmation.  

Cela en fait un choix solide pour tous les types de plateformes d’hébergement web, telles qu’un VPS ou l’hébergement web.  


# Nginx

Nginx 'engine x' est un serveur HTTP (Igor Sysoev) gratuit et open source. (racheté par F5 Networks mars 2019)  
- Solution au problème connu sous le nom de **c10k** dans Apache; Traitement des 10 000 connexions simultanément.   
- Fournit les fonctionnalités de base du serveur HTTP, telles que les fichiers statiques et indexés, l’architecture modulaire, prise en charge de TLS, etc.  
Nginx est devenu de plus en plus populaire en raison de son utilisation légère des ressources et de sa capacité à évoluer facilement avec un minimum de matériel.   
Il excelle dans la fourniture rapide de contenu statique et est conçu pour transmettre des requêtes dynamiques à d’autres logiciels mieux adaptés à ces objectifs.   

Utilisé par de nombreux sites web à forte visibilité tels que Netflix, Hulu, Pinterest et Airbnb.  
Toutefois, les organisations de taille plus réduite, Apache offre quelques avantages par rapport à Nginx, tels que sa configuration simple,
ses nombreux modules et son environnement convivial. 

## Arborescence Nginx ( version full )

![Arborescence Nginx](tree-nginx-full.png "Arborescence Nginx")



## Architecture Nginx

![Architecture Nginx](architecture_nginx.png "Nginx")

La principale différence entre NGINX et Apache, en termes de modèles d'événements, est que NGINX ne configure pas les processus de travail supplémentaires par connexion.
Dans la plupart des cas, la configuration NGINX recommandée exécute un processus de travail par CPU, ce qui maximise l'efficacité du matériel. 

##  Fonctionnalités

NGINX dispose également d'un ensemble complet de fonctionnalités et peut effectuer différents rôles de serveur:

- Un serveur proxy inverse pour le protocole HTTP, HTTPS, SMTP, POP3 et IMAP

- Un équilibreur de charge et un cache HTTP

- Un proxy de frontend pour Apache et d'autres serveurs Web, exemple la combinaison entre la flexibilité d'Apache et la bonne performance de contenu statique de NGINX

NGINX prend en charge les gestionnaires FastCGI et SCGI pour le service de scripts de contenu dynamique tels que PHP (PHP5-FPM) et Python.  
Il utilise la pile LEMP: Variante de LAMP en utilisant l'orthographe phonétique de NGINX (Linux, "Engine-X", MySQL, PHP).

## Les fichiers de configuration importants

- /etc/nginx/nginx.conf qui contient la configuration générale de nginx
- /etc/nginx/proxy_params les paramètres généraux de nginx proxy
- /etc/nginx/site-enabled et /etc/nginx/site-available qui contient la configuration des sites  
  --> fonctionnement assez similaire à Apache.  

## Les fichiers de logs par défaut : 

- /var/log/nginx/access.log  
- /var/log/nginx/error.log

## Declaration/activation d'un site 

Il faut que le fichier de configuration soit présent dans "/etc/nginx/site-enable"  
Par défaut, nginx lit tous les fichiers contenus dans ce répertoire, à la fin de /etc/nginx/nginx.conf se trouve ces inclusions :  

```conf
...
include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*;
```

Exemple :  
`ln -s /etc/nginx/sites-available/www.exemple.org.conf /etc/nginx/sites-enabled/`

et relancer Nginx ( nginx reload ) pour prendre en compte la nouvelle configuration 

## Exemple de configuration

```conf
server {
 listen 443;
 server_name www.monsite.fr;
 ssl on;
 ssl_certificate /etc/ssl/monsite.crt;
 ssl_certificate_key /etc/ssl/monsite.key;

access_log /var/logs/apache2/access.log;
error_log /var/logs/apache2/error.log;

root /home/www/site1;
...
}
```

# reverse proxy

![Reverse-proxy Nginx](proxyreverse.gif "Reverse-proxy")

## Exemple de configuration 

/etc/nginx/conf.d/nodeapp.conf 

```conf
server {
  listen 80;
  listen [::]:80;

  server_name example.com;

  location / {
      proxy_pass http://webserver1:3000/;
  }
}
```

## Autres proxy 

Apache, Lighttpd, Squid, Freeproxy et Microsoft ISA.  
Outre ces solutions dédiées, il existe des boîtiers de gestion du trafic qui remplissent également cette tâche comme ceux de BlueCoat, Deny All, CheckPoint, F5 Networks ou Cisco.  
Le choix d'un reverse proxy s'effectue en fonction du serveur Web majoritaire, Apache ou IIS, et de ses performances vis-à-vis de ce dernier.  

# Load balancing

3 Methodes de load balacing (repartition de charge) 

- round-robin — Les requetes  sont envoyées à tour de role aux serveurs  
- least-connected — Les prochaines requetes sont envoyé vers le serveur qui a le moins de connexions actives. 
- ip-hash — Répartit les requêtes en fonction d'une table de hachage basée sur les adresses IP d'où proviennent les requêtes.


## Exemple de la doc officielle

sans aucune precision : Methode round-robin  
```conf
upstream backend  {
  server backend1.example.com weight=5;
  server backend2.example.com:8080;
  server unix:/tmp/backend3;
}
 
server {
  location / {
    proxy_pass  http://backend;
  }
}
```

# Comparaison Apache / Nginx

|  -  |Apache|Nginx|  
|:---:|:---: |:---:|  
|Approche|Multithread pour traiter les requêtes des clients.|Evénementielle pour répondre aux requêtes des clients.|  
|Gestion du contenu dynamique| Gestion du contenu dynamique au sein du serveur Web lui-même.|Le contenu dynamique ne peut pas être géré nativement.|  
|Traitement de requêtes|Ne peut traiter plusieurs requêtes simultanément avec un trafic Web important.|Traitement simultané de plusieurs requêtes.|  

---------

|  -  |Apache|Nginx|  
|:---:|:---: |:---:|  
|Modules|Chargés ou déchargés dynamiquement, flexible.|Ne peuvent pas être chargés dynamiquement.Ils doivent être compilés|  
|Serveur Proxy|module mod_proxy|proxy et reverse-proxy.|  
|Thread|Un seul thread ne peut traiter qu’une seule connexion.|Un seul thread peut gérer plusieurs connexions.|  


# Exemple de cohabitation :


![Le meilleur des 2 ](nginxwithapache.png "Nginx et Apache")


# Conclusion

Apache et Nginx sont des serveurs Web performant permettant de répondre à des besoins différents en terme de charge, de fonctionnalités.  

Ils sont proches les uns des autres sur le plan conceptuel, mais sont des concurrents proches dans le secteur des serveurs Web. 

Apache est le leader de l’écosystème de serveurs Web depuis plus de 20 ans. 

Cependant, Nginx est devenu le standard pour les applications et les sites Web à fort traffic.


# Reference 

Documentation complète  
__Apache 2.4__  
https://httpd.apache.org/docs/2.4/fr/

__NGINX 1.16__  
https://nginx.org/en/docs/


# SOCRATIVE 

Student login 

salle de classe : SIERP2504

**TP**   
**Google classroom**  

Salle de classe :  
**mkkmllj** 


