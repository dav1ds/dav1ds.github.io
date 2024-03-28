# en-tête X-Frame-Options

## A quoi sert l'en tête X-FRAME-OPTIONS ?

La balise iframe est une balise permettant d'afficher sur une page web le contenu d'une autre page. 

L'utilisation d'iframe est une pratique courante mais ces balises peuvent être utilisées pour de mauvaises intentions et devenir ainsi un vecteur d'attaques. 

En effet, il est possible pour un pirate d'embarquer une page web dans un iframe et d'en modifier le comportement... Il pourra ainsi duper l'internaute et récupérer des identifiants et mots de passe par exemple.

L'en-tête X-FRAME-OPTIONS va permettre d'interdire l'inclusion des pages dans des iframe. La directive X-FRAME-OPTIONS peut prendre 3 valeurs :

- Deny : interdit tout chargement de la page à l'intérieur d'un iframe

- Sameorigin : l'inclusion est autorisée seulement si la page appelante possède la même origine c'est à dire, même domaine, même protocole et même port.

- Allow-from <url> :  permet d'autoriser explicitement des domaines à inclure la page dans un iframe.

## Implémenter l'en-tête X-FRAME-OPTIONS sur son site web

Activer l'en-tête X-FRAME-OPTIONS directement dans la configuration du serveur web.
Editer le fichier /etc/apache2/conf-available/security.conf pour ajouter la ligne ci-dessous :

Header set X-Frame-Options: "sameorigin"

Si pas d'accès à la conf web , possible de positionner cette ligne dans .htaccess



