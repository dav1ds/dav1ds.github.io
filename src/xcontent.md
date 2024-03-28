# X-Content-Type-Options

Securisation du serveur 

todo : HSTS et X-Frame-Options

Lorsqu'un serveur envoie une page web, il indique le type de données (script, image, etc.) via les types MIME au client afin que celui-ci interprète toutes ces données correctement. 
Ces types MIME sont intégrés dans des en-têtes content-type. Ces directives doivent être suivies par le client, c'est à dire le navigateur web qui a demandé la page. 

Or il existe une fonctionnalité sur les navigateurs actuels (Chrome, Internet Explorer...) qui permet de modifier les types MIME à la volée. Ainsi, le navigateur ne tient pas compte de la directive content-type envoyée par le serveur web et va modifier le type de données. 

Dès lors, on comprend que cette modification peut être dangereuse et constituer un risque : c'est ce qu'on appelle les attaques Drive-By-Download.

# Correctif serveur
Pour se protéger de ce changement de types MIME à la volée par le client, il existe un en-tête de trame : il s'agit de la directive X-Content-Type-Options. Cette directive ne peut prendre qu'un seul paramètre : nosniff. 


modifié le fichier security.conf se trouvant dans /etc/apache2/conf-available/ pour y ajouter la ligne ci-dessous :

Header set X-Content-Type-Options: "nosniff"

Ce réglage peut aussi être implémenté via le fichier htaccess de votre site. Il faudra le positionner dans la section appropriée : mod_headers.

<IfModule mod_headers.c>

Header always set X-Content-Type-Options "nosniff"

</IfModule>

